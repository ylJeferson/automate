<p align="center">
 <img width="150px" src="automate.ico" align="center" alt="Automate Icon"/>
 <h1 align="center">Automate</h1>
 <p align="center">Arquivo <strong>.bat</strong> para automatizar algumas instalações de programas no sistema operacional windows</p>
</p>

<p align="center">
    <a href="https://github.com/ylJeferson/automate">Download</a>
    ·
    <a href="https://github.com/ylJeferson/automate/issues/new/choose">Reportar Bug</a>
    ·
    <a href="https://github.com/ylJeferson/automate/issues/new/choose">Sugerir Ideias</a>
  </p>
<!-- <p align="center">Gostou do projeto? Por favor, considere realizar uma <a href="https://www.paypal.me/yljeferson">doação</a> para me ajudar! -->

## Menu

- [História](#história)
- [Customização](#customização)
  - [Opções](#opções)
  - [Configurações](#configurações)
  - [Programas](#programas)
<br>

## História

Começa com os meus 18 a 19 anos de idade.
Comecei a estagiar na prefeitura da minha cidade, e lá eles tinham alguns arquivos que automatizavam a instalação de alguns programas.
Porém estavam separados por sistema operacional e arquitetura do sistema.
Eu juntamente com o o outro estágiario "melquichiclete" fiz a mesclagem desses arquivos.
Além disso foram adicionado mais alguns programas que antes eram instalados manualmente.

_Nota: Após o fim do estágio eu continuei incrementando o arquivo e hoje em dia é possível criar vários perfis de instalação._
<br>

## Customização

1. Você pode estar criando até 10 perfis de instalação.
2. Para tanto basta setar a variável `PERFIL` com os números dos programas que você deseja instalar.
3. Para adicionar um programa à lista, basta criar uma label com um número de 10 a 99.
4. Utilize a opção `3` para baixar/atualizar os arquivos de instalação.
5. Para o bom funcionamento do batch quando em execução, é recomendado utilizar a encripitação `windows-1252`.
6. Também é feito o download de um arquivo compactado que contem um executar chamado `autopy-downloader.exe`, pode ser que seu antivírus reconheça como arquivo malicioso.

_Nota: Assim que realizar o download do batch, utilizar a opção 3 para que o proprio script realize o download dos prorgamas e arquivos de configuração._

### Opções:

 - `00 - Padronização:` Aplica configurações e executa alguns registros afim de padronizar o sistema.
 - `01 - Clientes:` Realiza a instalação de alguns programas previamente configurados diretamente no arquivos.
 - `02 - Do It Yourself:` Abre um menu com as opções de programas que você pode instalar.
      - _Basta digitar o número dos programas correspondentes e separar por espaço. (Exemplo: 11 24 30 ...)_
 - `03 - Update:` Faz a atualização dos programas e arquivos de configuração.
      - _Caso não for possível realizar algum download, ao final é aberto um bloco de notas listando todos os downloads que não foram realizados._

### Configurações:

 - `10 - Elevar Permissões:` Executa o arquivo batch como administrador.
 - `11 - Administrador:` Ativa o Administrador local da máquina.
      - _Você deve digitar uma senha para o usuário, ou simplesmente apertar o enter para confirmar a senha em branco_
 - `12 - Ponto de Restauração:` Cria um ponto de restauração no windows.
      - _Este comando só funciona uma vez por dia_
 - `13 - Estrutura de Pastas:` Cria uma estrutura de pastas no Disco Local C e da permissão para todos os usuários.
 - `14 - Copiar os Arquivos:` Copia alguns arquivos para a Área de Trabalho e para a pasta Config.
 - `15 - Firewall:` Desativa o Firewall do Windows.
 - `16 - Adaptador de Rede:` Renomeia o adaptador de rede principal para Ethernet.
 - `17 - Windows Update:` Desativa as atualizações pelo Windows Update.
      - _Você ainda pode atualizar manualmente clicando no botão "Verificar se há atualizações"_
 - `19 - Antivirus:` Cofigura a pasta "C:\config\" nas exclusões do Windows Defender.
 - `30 - PowerShell:` Habilita o PowerShell para enviar comando remotamente.
 - `90 - Padronização:` Cofigura fechamento de tampa e suspensão para não fazer nada.
 - `91 - Registros:` Executa alguns arquivos de registro e da permissão para alteração neles.
 - `98 - Arquivo Log:` Abre o arquivo de log listando nomes de programas que deram algum erro durante a instalação.
 - `99 - Reiniciar:` Reinicia o computador afim de aplicar atualizações ou configurações de programas recém instalados.

### Programas:

 - `18 - 7-Zip:` /S
 - `20 - Any Desk:` --install "C:\Program Files\AnyDesk" --start-with-win --create-shortcuts --create-desktop-icon --silent
 - `21 - Avast:` /silent 
 - `22 - Folder Size:` /qb
 - `23 - Google Chrome:` _Não há parâmetros_
 - `24 - Java:` /s
 - `25 - K-Lite:` /silent
 - `26 - Libre Office:` /qb REGISTER_ALL_MSO_TYPES=0 UI_LANGS=pt_BR ISCHECKFORPRODUCTUPDATES=0 REBOOTYESNO=No QUICKSTART=0 ADDLOCAL=ALL VC_REDIST=0
 - `27 - Light Shot:` /sp /silent /supressmsgboxes
 - `28 - Microsoft Office:` _Não há parâmetros_
 - `29 - Office 16.013801.20266:` /update user updatetoversion=16.0.13801.20266
 - `31 - TeamViewer:` /S
 - `32 - UltraVNC:` /verysilent /supressmsgboxes /nocancel /norestart /loadinf=".\configuracoes\programas\uvnc\ultravnc.inf"
 - `33 - VCRedist:` /install /quiet /norestart
 - `34 - Winrar:` /S

_Nota:  Acima temos os programas que podem ser isntalados e seus parâmetros para uma instalação silenciosa._
<br>

