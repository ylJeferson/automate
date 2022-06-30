<p align="center">
 <img width="100px" src="automate.ico" align="center" alt="Automate Icon"/>
 <h2 align="center">Automate</h2>
 <p align="center">Batch file para automatizar algumas instalações de programas no windows</p>
</p>

<p align="center">
    <a href="https://github.com/ylJeferson/automate">Download</a>
    ·
    <a href="https://github.com/ylJeferson/automate/issues/new/choose">Report Bug</a>
  </p>
<!-- <p align="center">Love the project? Please consider <a href="https://www.paypal.me/yljeferson">donating</a> to help it improve! -->

# Menu

- [Automate](#automate)
- [Customização](#customização)
  - [Opções](#opções)
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
4. Utilize a opção 8 para baixar/atualizar os arquivos de instalação.
5. 

_Nota: Recomendo utilizar apenas programas com instalação silenciosa._

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
<!--
 - `bgcolor` - Banner background _(any css color)_
 - `name` - Title _(whatever title you want)_. (I recommend it be your name)
 - `namecolor` - Title color _(any css color)_.  (I recommend it be hex color)
 - `namefont` - Title Font _(google font name)_. (I recommend https://fonts.google.com/)
 - `namefontsize` - Title _(font-size)_. (I recommend using 'rem')
 - `anim` - Words _(any words you want, separated by ';')_. (I recommend it be your qualities)
 - `animcolor` - Words color _(any css color)_.  (I recommend it be hex color)
 - `animfont` - Words Font _(google font name)_. (I recommend https://fonts.google.com/)
 - `animfontsize` - Words _(font-size)_. (I recommend using 'rem')
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
