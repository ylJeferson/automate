# Iniciar: referencia de reparo de permissao

Registro documental de 28/10/2026 para [verificacoes futuras](../verificacoes_futuras.md#correcoes-fix). **Nao integrado nem executado nesta tarefa.** Nao e uma correcao universal para o Iniciar.

## Fontes preservadas

- Versao legivel: `C:\config\codex\chats\Manutenção\correcoes\iniciar_permissoes.ps1`.
- SHA-256: `BFA2A0BB22EEE48C96C65069B74539845D83379E3F029249330D1501EEBF62F0`.
- Wrapper completo: `C:\Users\yljef\Documents\Codex\2026-10-22\es\outputs\corrigir_iniciar_powershell.txt`.
- SHA-256: `3627F8A942D1B27FA3B28B6456B8FED122F5E2817F8984773321E65DBA8AC23B`.

Os hashes identificam os arquivos de origem, incluindo seus finais de linha. O codigo abaixo preserva o conteudo, sem transportar dados da conta Bruna. O SID fixo e o do pacote do Iniciar, nao de uma conta de usuario.

## Limites e validacao historica

- O wrapper tem uma unica linha de 5.081 caracteres, sem contar a quebra final, abaixo do limite de linha do CMD; nao ha aspas duplas internas, `%` ou `!`.
- Segundo o historico recebido, houve validacao no CMD com delayed expansion habilitada usando uma copia com `$verificar = $true`, somente leitura, no Windows PowerShell 5.1. Nao houve aplicacao de permissoes neste computador nesse teste.
- As duas versoes abaixo preservam `$verificar = $false` da fonte: **executa-las pode alterar a ACL e parar o host do Iniciar**. A autorizacao atual abrange somente este registro.
- O wrapper e para CMD/BAT. Em PowerShell ja aberto, as variaveis podem ser expandidas antes de chegar ao comando interno.
- Manter `Get-Acl/Set-Acl -Path` para o alvo sem curingas; `Get-Acl -LiteralPath` falhou na tentativa original.
- A chave precisa existir. O codigo nao altera `ShowRecentList`, nao redefine ACLs globais e adiciona somente `ReadKey,SetValue` para o pacote, sem heranca para filhos.
- O backup `.reg` guarda valores, nao ACL. A permissao original fica em `permissoes_antes.sddl`; `alvo.json` identifica o alvo.

## Versao legivel

```powershell
$ErrorActionPreference = 'Stop'
Import-Module (Join-Path $PSHOME 'Modules\Microsoft.PowerShell.Security\Microsoft.PowerShell.Security.psd1') -ErrorAction Stop
$verificar = $false
$identidade = [Security.Principal.WindowsIdentity]::GetCurrent()
$administrador = New-Object Security.Principal.WindowsPrincipal($identidade)
if (-not $verificar -and -not $administrador.IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)) {
    throw 'Execute no CMD como administrador, na sessao do usuario afetado.'
}
$sessao = (Get-Process -Id $PID).SessionId
$usuarios = @(Get-CimInstance Win32_Process -Filter 'Name = ''explorer.exe''' |
    Where-Object {$_.SessionId -eq $sessao} |
    ForEach-Object {
        $dono = Invoke-CimMethod -InputObject $_ -MethodName GetOwnerSid
        if ($dono.ReturnValue -ne 0) {throw 'Nao foi possivel identificar o dono do Explorer.'}
        $dono.Sid
    } | Sort-Object -Unique)
if ($usuarios.Count -ne 1) {throw 'Usuario da sessao nao identificado com seguranca.'}
$usuario = [string]$usuarios[0]
$perfis = @(Get-CimInstance Win32_UserProfile |
    Where-Object {$_.SID -eq $usuario -and $_.Loaded -and -not $_.Special})
if ($perfis.Count -ne 1) {throw 'Perfil carregado do usuario nao encontrado.'}
if ($usuario -eq $identidade.User.Value) {
    $pacotes = @(Get-AppxPackage -Name Microsoft.Windows.StartMenuExperienceHost)
} else {
    $pacotes = @(Get-AppxPackage -User $usuario -Name Microsoft.Windows.StartMenuExperienceHost)
}
if ($pacotes.Count -ne 1 -or $pacotes[0].PackageFamilyName -ne 'Microsoft.Windows.StartMenuExperienceHost_cw5n1h2txyewy') {
    throw 'Pacote do Iniciar esperado nao encontrado para esse usuario.'
}
$subchave = 'Software\Microsoft\Windows\CurrentVersion\Start'
$chave = 'Registry::HKEY_USERS\' + $usuario + '\' + $subchave
$acl = Get-Acl -Path $chave
$pacoteSid = [Security.Principal.SecurityIdentifier]::new('S-1-15-2-515815643-2845804217-1874292103-218650560-777617685-4287762684-137415000')
$direitos = [Security.AccessControl.RegistryRights]'ReadKey,SetValue'
$regras = @($acl.GetAccessRules($true,$true,[Security.Principal.SecurityIdentifier]))
$aplicaveis = @($regras | Where-Object {
    ($_.PropagationFlags -band [Security.AccessControl.PropagationFlags]::InheritOnly) -eq 0
})
$negacoes = @($aplicaveis | Where-Object {
    $_.AccessControlType -eq 'Deny' -and ($_.RegistryRights -band $direitos) -ne 0
})
if ($negacoes.Count) {throw 'Existe regra Deny nesta chave. Revise a ACL antes de aplicar.'}
$permitidos = 0
foreach ($regra in $aplicaveis) {
    if ($regra.IdentityReference.Value -eq $pacoteSid.Value -and $regra.AccessControlType -eq 'Allow') {
        $permitidos = $permitidos -bor [int]$regra.RegistryRights
    }
}
if ($verificar) {
    Write-Host ('VERIFICACAO: usuario=' + $usuario + '; sessao=' + $sessao + '; chave=' + $chave)
    Write-Host ('Permissao especifica ja presente: ' + (($permitidos -band [int]$direitos) -eq [int]$direitos))
    return
}
if (($permitidos -band [int]$direitos) -eq [int]$direitos) {
    Write-Host 'A permissao especifica ja existe. Nenhuma alteracao aplicada.'
    return
}
$backup = Join-Path $perfis[0].LocalPath ('backup_permissao_iniciar_' + (Get-Date -Format yyyyMMdd_HHmmss) + '_' + [guid]::NewGuid().ToString('N').Substring(0,6))
New-Item -ItemType Directory -Path $backup | Out-Null
$sddl = Join-Path $backup 'permissoes_antes.sddl'
[IO.File]::WriteAllText($sddl,$acl.Sddl,[Text.Encoding]::UTF8)
if ([IO.File]::ReadAllText($sddl) -ne $acl.Sddl) {throw 'Falha ao verificar o backup da permissao.'}
$registro = Join-Path $backup 'start_antes.reg'
& (Join-Path $env:SystemRoot 'System32\reg.exe') export ('HKU\' + $usuario + '\' + $subchave) $registro /y | Out-Null
if ($LASTEXITCODE -ne 0 -or -not (Test-Path -LiteralPath $registro -PathType Leaf)) {
    throw 'Backup dos valores falhou; permissao nao alterada.'
}
$alvo = [pscustomobject]@{UsuarioSid=$usuario;Chave=$chave;PacoteSid=$pacoteSid.Value;Direitos='ReadKey,SetValue'}
[IO.File]::WriteAllText((Join-Path $backup 'alvo.json'),($alvo | ConvertTo-Json),[Text.Encoding]::UTF8)
$acl.AddAccessRule([Security.AccessControl.RegistryAccessRule]::new($pacoteSid,$direitos,[Security.AccessControl.AccessControlType]::Allow))
Set-Acl -Path $chave -AclObject $acl
$apos = Get-Acl -Path $chave
$confirmado = @($apos.GetAccessRules($true,$true,[Security.Principal.SecurityIdentifier]) | Where-Object {
    $_.IdentityReference.Value -eq $pacoteSid.Value -and $_.AccessControlType -eq 'Allow' -and
    ($_.RegistryRights -band $direitos) -eq $direitos -and
    ($_.PropagationFlags -band [Security.AccessControl.PropagationFlags]::InheritOnly) -eq 0
})
if (-not $confirmado.Count) {throw ('Nao foi possivel confirmar a permissao aplicada. Backup: ' + $backup)}
Write-Host ('PERMISSAO APLICADA. Backup: ' + $backup)
Get-Process StartMenuExperienceHost -ErrorAction SilentlyContinue |
    Where-Object {$_.SessionId -eq $sessao} | Stop-Process -Force -ErrorAction SilentlyContinue
Write-Host 'Clique no Iniciar para testar.'
```

## Linha literal completa para CMD/BAT

Referencia para avaliacao futura, sem dependencia de arquivo PS1. Manter como uma unica linha:

```bat
powershell "$ErrorActionPreference = 'Stop'; Import-Module (Join-Path $PSHOME 'Modules\Microsoft.PowerShell.Security\Microsoft.PowerShell.Security.psd1') -ErrorAction Stop; $verificar = $false; $identidade = [Security.Principal.WindowsIdentity]::GetCurrent(); $administrador = New-Object Security.Principal.WindowsPrincipal($identidade); if (-not $verificar -and -not $administrador.IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)) {     throw 'Execute no CMD como administrador, na sessao do usuario afetado.'; }; $sessao = (Get-Process -Id $PID).SessionId; $usuarios = @(Get-CimInstance Win32_Process -Filter 'Name = ''explorer.exe''' |     Where-Object {$_.SessionId -eq $sessao} |     ForEach-Object {         $dono = Invoke-CimMethod -InputObject $_ -MethodName GetOwnerSid;         if ($dono.ReturnValue -ne 0) {throw 'Nao foi possivel identificar o dono do Explorer.'};         $dono.Sid;     } | Sort-Object -Unique); if ($usuarios.Count -ne 1) {throw 'Usuario da sessao nao identificado com seguranca.'}; $usuario = [string]$usuarios[0]; $perfis = @(Get-CimInstance Win32_UserProfile |     Where-Object {$_.SID -eq $usuario -and $_.Loaded -and -not $_.Special}); if ($perfis.Count -ne 1) {throw 'Perfil carregado do usuario nao encontrado.'}; if ($usuario -eq $identidade.User.Value) {     $pacotes = @(Get-AppxPackage -Name Microsoft.Windows.StartMenuExperienceHost); } else {     $pacotes = @(Get-AppxPackage -User $usuario -Name Microsoft.Windows.StartMenuExperienceHost); }; if ($pacotes.Count -ne 1 -or $pacotes[0].PackageFamilyName -ne 'Microsoft.Windows.StartMenuExperienceHost_cw5n1h2txyewy') {     throw 'Pacote do Iniciar esperado nao encontrado para esse usuario.'; }; $subchave = 'Software\Microsoft\Windows\CurrentVersion\Start'; $chave = 'Registry::HKEY_USERS\' + $usuario + '\' + $subchave; $acl = Get-Acl -Path $chave; $pacoteSid = [Security.Principal.SecurityIdentifier]::new('S-1-15-2-515815643-2845804217-1874292103-218650560-777617685-4287762684-137415000'); $direitos = [Security.AccessControl.RegistryRights]'ReadKey,SetValue'; $regras = @($acl.GetAccessRules($true,$true,[Security.Principal.SecurityIdentifier])); $aplicaveis = @($regras | Where-Object {     ($_.PropagationFlags -band [Security.AccessControl.PropagationFlags]::InheritOnly) -eq 0; }); $negacoes = @($aplicaveis | Where-Object {     $_.AccessControlType -eq 'Deny' -and ($_.RegistryRights -band $direitos) -ne 0; }); if ($negacoes.Count) {throw 'Existe regra Deny nesta chave. Revise a ACL antes de aplicar.'}; $permitidos = 0; foreach ($regra in $aplicaveis) {     if ($regra.IdentityReference.Value -eq $pacoteSid.Value -and $regra.AccessControlType -eq 'Allow') {         $permitidos = $permitidos -bor [int]$regra.RegistryRights;     }; }; if ($verificar) {     Write-Host ('VERIFICACAO: usuario=' + $usuario + '; sessao=' + $sessao + '; chave=' + $chave);     Write-Host ('Permissao especifica ja presente: ' + (($permitidos -band [int]$direitos) -eq [int]$direitos));     return; }; if (($permitidos -band [int]$direitos) -eq [int]$direitos) {     Write-Host 'A permissao especifica ja existe. Nenhuma alteracao aplicada.';     return; }; $backup = Join-Path $perfis[0].LocalPath ('backup_permissao_iniciar_' + (Get-Date -Format yyyyMMdd_HHmmss) + '_' + [guid]::NewGuid().ToString('N').Substring(0,6)); New-Item -ItemType Directory -Path $backup | Out-Null; $sddl = Join-Path $backup 'permissoes_antes.sddl'; [IO.File]::WriteAllText($sddl,$acl.Sddl,[Text.Encoding]::UTF8); if ([IO.File]::ReadAllText($sddl) -ne $acl.Sddl) {throw 'Falha ao verificar o backup da permissao.'}; $registro = Join-Path $backup 'start_antes.reg'; & (Join-Path $env:SystemRoot 'System32\reg.exe') export ('HKU\' + $usuario + '\' + $subchave) $registro /y | Out-Null; if ($LASTEXITCODE -ne 0 -or -not (Test-Path -LiteralPath $registro -PathType Leaf)) {     throw 'Backup dos valores falhou; permissao nao alterada.'; }; $alvo = [pscustomobject]@{UsuarioSid=$usuario;Chave=$chave;PacoteSid=$pacoteSid.Value;Direitos='ReadKey,SetValue'}; [IO.File]::WriteAllText((Join-Path $backup 'alvo.json'),($alvo | ConvertTo-Json),[Text.Encoding]::UTF8); $acl.AddAccessRule([Security.AccessControl.RegistryAccessRule]::new($pacoteSid,$direitos,[Security.AccessControl.AccessControlType]::Allow)); Set-Acl -Path $chave -AclObject $acl; $apos = Get-Acl -Path $chave; $confirmado = @($apos.GetAccessRules($true,$true,[Security.Principal.SecurityIdentifier]) | Where-Object {     $_.IdentityReference.Value -eq $pacoteSid.Value -and $_.AccessControlType -eq 'Allow' -and     ($_.RegistryRights -band $direitos) -eq $direitos -and     ($_.PropagationFlags -band [Security.AccessControl.PropagationFlags]::InheritOnly) -eq 0; }); if (-not $confirmado.Count) {throw ('Nao foi possivel confirmar a permissao aplicada. Backup: ' + $backup)}; Write-Host ('PERMISSAO APLICADA. Backup: ' + $backup); Get-Process StartMenuExperienceHost -ErrorAction SilentlyContinue |     Where-Object {$_.SessionId -eq $sessao} | Stop-Process -Force -ErrorAction SilentlyContinue; Write-Host 'Clique no Iniciar para testar.'"
```
