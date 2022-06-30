<p align="center">
 <img width="150px" src="automate.ico" align="center" alt="Automate Icon"/>
 <h2 align="center">Automate</h2>
 <p align="center">Arquivo <strong>.bat</strong> para automatizar algumas instalações de programas no sistema operacional windows</p>
</p>

<p align="center">
    <a href="https://github.com/ylJeferson/automate">Download</a>
    ·
    <a href="https://github.com/ylJeferson/automate/issues/new/choose">Reportar Bug</a>
  </p>
<!-- <p align="center">Love the project? Please consider <a href="https://www.paypal.me/yljeferson">donating</a> to help it improve! -->

# Menu

- [Automate](#automate)
- [Customização](#customização)
  - [Opções](#opções)
  - [Configurações](#configurações)
  - [Programas](#programas)
<br>

## Automate

A história deste batch file começa com os meus 18-19 anos de idade.
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

<!--
 if %errorlevel% equ 1 set PERFIL=10 12 13 14 19 20 21 22 23 24 25 26 27 31 33 34 90 91 98<br>
 if %errorlevel% equ 2 set PERFIL=09 & rem do it yourself<br>
 if %errorlevel% equ 3 set PERFIL=bu & rem bat update<br>
 if %errorlevel% equ 4 cls && goto copyright<br>
 if %errorlevel% equ 5 cls && goto copyright<br>
 if %errorlevel% equ 6 cls && goto copyright<br>
 if %errorlevel% equ 7 cls && goto copyright<br>
 if %errorlevel% equ 8 cls && goto copyright<br>
 if %errorlevel% equ 9 cls && goto copyright<br>
 if %errorlevel% equ 10 set PERFIL=13 14 90 91 98<br>
-->
  
### Opções:

 - `00 - Padronização` - Aplica configurações e executa alguns registros afim de padronizar o sistema.
 - `01 - Clientes` - Realiza a instalação de alguns programas previamente configurados diretamente no arquivos.
 - `02 - Do It Yourself` - Abre um menu com as opções de programas que você pode instalar.
      - _Basta digitar o número dos programas correspondentes e separar por espaço. (Exemplo: 11 24 30 ...)_
 - `03 - Update` - Faz a atualização dos programas e arquivos de configuração.
      - _Caso não for possível realizar algum download, ao final é aberto um bloco de notas listando todos os downloads que não foram realizados._

### Configurações:
<!--
 - `bgcolor` - bgcolor=_transparent_
 - `name` - name=_Example_
 - `namecolor` - namecolor=_rgb%28255,99,71%29_
 - `namefont` - namefont=_Tangerine_
 - `namefontsize` - namefontsize=_10rem_
 - `anim` - anim=_that's;what;this;is_
 - `animcolor` - animcolor=_%236941d3_
 - `animfont` - animfont=_Flow%20Rounded_
 - `animfontsize` - animfontsize=_5em_
-->
### Programas:
<!--
 - `bgcolor` - bgcolor=_transparent_
 - `name` - name=_Example_
 - `namecolor` - namecolor=_rgb%28255,99,71%29_
 - `namefont` - namefont=_Tangerine_
 - `namefontsize` - namefontsize=_10rem_
 - `anim` - anim=_that's;what;this;is_
 - `animcolor` - animcolor=_%236941d3_
 - `animfont` - animfont=_Flow%20Rounded_
 - `animfontsize` - animfontsize=_5em_
-->
