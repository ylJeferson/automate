# Verificacoes futuras

Registro de 28/10/2026, encaminhado pelo chat Manutencao. Este catalogo documenta trabalho futuro; nao e uma diretriz permanente, uma integracao ao Automate nem uma autorizacao para executar os comandos.

As fontes foram lidas integralmente como dados. Nenhuma rotina foi executada neste registro. `automate.bat`, `G:\Automate.bat` e `guideline.md` nao fazem parte desta alteracao.

## Otimizacao de desempenho

**Situacao:** segundo o relato recebido, o tecnico aplicou a rotina no notebook da Bruna e estava reiniciando para testar. A validacao apos reiniciar permanece **PENDENTE**. A avaliacao e eventual integracao ao Automate tambem estao pendentes.

### Fontes

- Arquivo completo: `C:\config\codex\chats\Manutenção\otimizacao\otimizar_notebook.bat`.
- Copia de entrega: `C:\Users\yljef\Documents\Codex\2026-10-22\es\outputs\otimizar_notebook.bat`.
- SHA-256 de ambos, conferido neste registro: `2B8695A92687856EE39CD38416A881E803A0F3BD64C6BC943BA29DAF99F6B46E`.

### Catalogo de rotinas

| Area | Comandos, funcoes e comportamento a avaliar |
| --- | --- |
| Alvo e modos | `Assert-Target` exige a conta/perfil Bruna e um SID fixo, alem de administrador. A aplicacao verifica build de Windows 10 2004-22H2. Modos: `/aplicar`, `/restaurar`, `/verificar`, `/ajuda`, `/help` e `/?`; `Show-Plan` apresenta o escopo. `/verificar` e somente leitura, sem ajustes, limpeza ou backup. |
| Plano de energia | `Get-PowerGuid` consulta `/getactivescheme`. Antes dos ajustes, `powercfg /export` salva `plano_anterior.pow`. `Apply-Power` usa `/duplicatescheme` para clonar Alto desempenho (`8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c`), com fallback para clonar o plano atual; `/changename` nomeia o plano proprio e `/setactive` o ativa. |
| Parametros de energia | `/setacvalueindex` e `/setdcvalueindex` configuram `SUB_PROCESSOR PROCTHROTTLEMIN 100`, `SUB_PROCESSOR PROCTHROTTLEMAX 100`, refrigeracao ativa (`94d3a615-a899-4ac5-ae2b-e4d8f634367f=1`, quando suportada) e `SUB_PCIEXPRESS ASPM 0`. CPU minima/maxima de 100% em tomada e bateria atendem ao pedido explicito de priorizar desempenho sem preocupacao com consumo; nao sao um padrao ja aprovado para o Automate. |
| Efeitos via API | `Initialize-UiApi`, `Get-UiSnapshot`, `Set-UiPerformance` e `Restore-UiSnapshot` usam `SystemParametersInfoW`. Alvos: `UIEffects=false`, `ClientAreaAnimation=false`, `DragFullWindows=false`, `MenuShowDelay=100` e `MinAnimate=0`, com persistencia e notificacao. A suavizacao de fontes e preservada. |
| Efeitos via Registro | `Get-Settings` define `VisualFXSetting=2`, `TaskbarAnimations=0`, `EnableAeroPeek=0` e `EnableTransparency=0` na conta. |
| Edge | Somente se o executavel estiver instalado nos caminhos verificados de Program Files: politicas da conta `StartupBoostEnabled=0` e `BackgroundModeEnabled=0`. |
| Paginacao | `Get-CimInstance Win32_ComputerSystem` e `Set-CimInstance -Property @{AutomaticManagedPagefile=$true}` habilitam o gerenciamento automatico quando necessario. O estado anterior de `AutomaticManagedPagefile` e o valor tipado `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PagingFiles` entram no backup. |
| Inicializacao | `Get-StartupCandidates` e `Select-Startup` oferecem selecao explicita de entradas conhecidas em `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` e `HKCU\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Run`. Incluem candidatos como OneDrive, Edge, Teams, Discord, Spotify, Steam, Epic, BlueStacks/HD-Agent e atualizadores Adobe. Valores e tipos sao salvos antes de remover somente as entradas escolhidas; ENTER mantem todas. Os programas nao sao desinstalados. |
| Temporarios | `Clean-TemporaryRoot` limita a limpeza a arquivos com ultima gravacao ha mais de sete dias em `AppData\Local\Temp` da conta e `%SystemRoot%\Temp` (`Windows\Temp`). Valida os caminhos e seus limites, nao segue pontos de analise e ignora o proprio BAT. Arquivos inacessiveis sao contabilizados. A limpeza nao e revertida. |
| Componentes | `DISM /Online /Cleanup-Image /StartComponentCleanup /NoRestart`, sem `/ResetBase`. Trata retornos `0` e `3010`. A limpeza de componentes nao e revertida pelo restaurador. |
| Armazenamento | `Optimize-SystemDisk` consulta SMART via `MSStorageDriver_FailurePredictStatus` em `root/wmi`, quando disponivel. Em alerta explicito `PredictFailure`, ignora a otimizacao. Caso contrario, usa `defrag <unidadeSistema> /O /H /U /V`. SMART indisponivel nao confirma saude do disco. |
| Backup tipado | `Open-BaseKey`, `Get-RegSnapshot`, `Set-RegData`, `Remove-RegValue` e `Restore-RegSnapshot` preservam existencia, tipo e valor original, incluindo ausencia de valor e strings expansivas sem expansao. `estado.json` registra usuario, computador, versao, planos, Registro, inicializacao, efeitos de UI e paginacao. O estado e relido para verificacao antes dos ajustes. |
| Restauracao | A copia integral do BAT em `restaurar_otimizacao.bat` e autossuficiente e reconhece seu nome. `Restore-Optimization` valida pasta, usuario, maquina, versao, GUIDs e alvos permitidos. Restaura plano anterior (`/setactive`, com `/import` do `.pow` se necessario), remove somente o plano criado (`/delete`), restaura paginacao, UI e valores/inicializacao. Nao recupera limpezas de arquivos ou componentes. |
| Logs e controle | `Write-Log`, `Invoke-Native` e `Invoke-Step` registram etapas, saidas, retornos e avisos em `otimizacao.log` ou `restauracao.log`. Retornos gerais: `0` sucesso, `2` avisos e `1` erro. O reinicio e manual. |

### Validacao e limites

- [ ] Obter o resultado do teste da Bruna apos reiniciar, incluindo Iniciar, Wi-Fi, audio e programas. Nao marcar como concluido apenas porque a rotina foi aplicada.
- [ ] Avaliar desempenho e impacto dos ajustes; **nao houve benchmark** informado.
- [ ] Adaptar a trava de SID/perfil especifica da Bruna antes de eventual integracao, sem copiar cegamente o BAT para producao.
- [ ] Avaliar compatibilidade por versao do Windows, opcoes de energia suportadas, avisos/logs e restauracao, inclusive os efeitos nao reversiveis das limpezas.

Segundo o historico recebido, sintaxe, modo de consulta e recusa de aplicacao em outra conta foram testados no Windows PowerShell 5.1. Isso nao equivale a validar o resultado apos reiniciar o notebook da Bruna ou uma versao integrada ao Automate.

Nao foram incluidos: desativacao de Defender/Windows Update, alteracoes genericas de servicos, remocao de aplicativos, hacks de BCD/HPET/RAM, alteracoes de ACLs ou reconfiguracao do Iniciar. O reparo abaixo e uma referencia separada.

## Correcoes (Fix)

### Iniciar: permissao de gravacao na chave Start

**Situacao:** avaliacao e integracao futuras **PENDENTES**. A causa especifica foi resolvida no caso Bruna, segundo o historico; o wrapper generalizado foi testado no CMD apenas em modo de verificacao, sem aplicar permissoes neste computador.

**Evidencia historica:** dump com `0xc000027b` contendo `0x80070005`, em `StartUI!GlobalStartData::CopyCDSDataToReg`, `GlobalStartData.cpp(94)`, ao gravar `ShowRecentList` em `Software\Microsoft\Windows\CurrentVersion\Start` com `KEY_SET_VALUE`.

**Reparo delimitado:** adicionar somente `ReadKey|SetValue` para o SID do pacote do Iniciar, na chave existente, sem heranca/filhos e preservando as demais ACEs, o proprietario e a protecao. Nao alterar valores `ShowRecentList`, criar uma chave ausente ou redefinir ACLs globais. Nao afirmar que corrige qualquer defeito do Iniciar.

O [documento de apoio](correcoes/iniciar_permissoes.md) preserva a versao PowerShell legivel, a linha literal completa `powershell "..."` para CMD/BAT, as referencias de origem, hashes e cuidados de uso. O codigo e generico quanto a usuario/maquina com pacote e chave compativeis; nao contem o SID nem o nome de usuario da Bruna.

Comportamento a preservar na avaliacao:

- Identifica o dono do `explorer.exe` da mesma sessao com `GetOwnerSid`; exige um unico SID/perfil carregado e nao especial.
- Usa `HKEY_USERS\SID`, nao `HKCU`, para suportar elevacao por outra conta administrativa; confirma o pacote `Microsoft.Windows.StartMenuExperienceHost_cw5n1h2txyewy` e le a chave existente.
- Ignora o reparo quando a permissao especifica ja existe. Aborta para revisao se encontrar ACE `Deny` de leitura/gravacao aplicavel a chave (nao apenas as do SID do pacote).
- Salva SDDL, faz `reg export` e confere os backups antes de adicionar `ReadKey,SetValue` ao SID do pacote `S-1-15-2-515815643-2845804217-1874292103-218650560-777617685-4287762684-137415000`.
- Confere o grant depois de `Set-Acl` e reinicia somente o `StartMenuExperienceHost` da mesma sessao.

Verificacoes futuras:

- [ ] Avaliar aplicabilidade somente para essa causa, com pacote/chave compativeis e diagnostico de acesso negado.
- [ ] Validar alvo em cenario de elevacao por outra conta, idempotencia e abortos por `Deny`, chave ausente ou identificacao ambigua.
- [ ] Validar preservacao de ACEs, proprietario, protecao e ausencia de heranca para filhos; conferir backups e restauracao pelo SDDL.
- [ ] Validar eventual adaptacao ao BAT com delayed expansion e o reinicio limitado a mesma sessao, sem alterar a producao neste registro.

**Cuidados:** manter `Get-Acl/Set-Acl -Path` nesse alvo sem curingas; a forma original com `Get-Acl -LiteralPath` falhou. `.reg` nao restaura ACL: a permissao original esta no SDDL. O wrapper se destina a CMD/BAT; cola-lo em um PowerShell ja aberto pode expandir variaveis antes da execucao. A copia documental tem `$verificar = $false` como a fonte original e **nao deve ser executada como parte deste registro**.
