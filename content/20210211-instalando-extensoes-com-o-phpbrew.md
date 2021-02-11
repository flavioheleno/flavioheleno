# Instalando extensões com o phpbrew

No [texto anterior](content/20201128-instalando-o-php-8-no-macos-com-brew-e-phpbrew.md) foi abordado como fazer a instalação do
PHP 8 e uma primeira extensão, neste texto serão apresentadas outras extensões bem como suas dependências.

## Iconv

De acordo com o [manual](https://www.php.net/manual/en/intro.iconv.php) do PHP, iconv é uma extensão que permite
converter a codificação de strings através de uma série de funções.

Antes de mais nada, é necessário instalar a **libiconv**, usando o comando abaixo:

```bash
$ brew install libiconv
```

Mais uma vez, será importante verificar o local de instalação da libiconv, usando o brew conforme abaixo:

```bash
$ brew info libiconv
libiconv: stable 1.16 (bottled) [keg-only]
Conversion library
https://www.gnu.org/software/libiconv/
/usr/local/Cellar/libiconv/1.16 (30 files, 2.4MB)
  Poured from bottle on 2020-10-08 at 16:44:15
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/libiconv.rb
License: GPL-3.0
==> Caveats
libiconv is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have libiconv first in your PATH run:
  echo 'export PATH="/usr/local/opt/libiconv/bin:$PATH"' >> /Users/flavioheleno/.bash_profile

For compilers to find libiconv you may need to set:
  export LDFLAGS="-L/usr/local/opt/libiconv/lib"
  export CPPFLAGS="-I/usr/local/opt/libiconv/include"

==> Analytics
install: 4,940 (30 days), 15,200 (90 days), 67,622 (365 days)
install-on-request: 3,240 (30 days), 10,006 (90 days), 44,659 (365 days)
build-error: 0 (30 days)

```

Com o local de instalação identificado, pode-se finalmente instalar a extensão, usando o comando a seguir:

```bash
$ phpbrew ext install iconv -- --with-iconv=/usr/local/opt/libiconv
```

## Amqp

De acordo com o [repositório](https://github.com/php-amqp/php-amqp) da extensão, amqp é uma extensão que permite a
comunicação com qualquer servidor compatível com o protocolo AMQP.

Antes de mais nada, é necessário instalar o pacote **rabbitmq-c**, usando o comando abaixo:

```bash
$ brew install rabbitmq-c
```

Mais uma vez, será importante verificar o local de instalação do rabbitmq-c, usando o brew conforme abaixo:

```bash
$ brew info rabbitmq-c
rabbitmq-c: stable 0.10.0 (bottled), HEAD
C AMQP client library for RabbitMQ
https://github.com/alanxz/rabbitmq-c
/usr/local/Cellar/rabbitmq-c/0.10.0 (20 files, 489.4KB) *
  Poured from bottle on 2020-10-16 at 10:24:04
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/rabbitmq-c.rb
License: MIT
==> Dependencies
Build: cmake ✘, pkg-config ✔
Required: openssl@1.1 ✔, popt ✔
==> Options
--HEAD
  Install HEAD version
==> Analytics
install: 493 (30 days), 1,532 (90 days), 6,515 (365 days)
install-on-request: 465 (30 days), 1,453 (90 days), 6,045 (365 days)
build-error: 0 (30 days)
```

Com o local de instalação identificado, pode-se finalmente instalar a extensão, usando o comando a seguir:

```bash
$ phpbrew ext install amqp -- --with-librabbitmq-dir=/usr/local/Cellar/rabbitmq-c/0.10.0
```

## GD

De acordo com o [manual](https://www.php.net/manual/en/intro.image.php) do PHP, GD é uma extensão que dá acesso ao
desenvolvedor à funções para criar e manipular arquivos de imagem.

Neste caso, é importante ressaltar que a própria [libgd](http://www.libgd.org) depende de várias outras bibliotecas para
dar suporte a diferentes formatos de imagem, como [jpeg](http://www.ijg.org),
[png](http://www.libpng.org/pub/png/libpng.html) etc. Note também que a partir do 
[PHP 7.4.0](https://www.php.net/ChangeLog-7.php#PHP_7_4), a libpng e a zlib se tornaram requerimentos padrão da extensão.

Antes de mais nada, é necessário instalar as seguintes dependências: **libgd**, **libpng** e **zlib**. Instalaremos 
também o pacote **jpeg** para dar suporte a imagens no formato jpeg.

Para isso, bastar usar o comando abaixo:

```bash
$ brew install libgd libpng zlib jpeg
```

Desta vez, será importante verificar o local de instalação da zlib e do jpeg, uma vez que o phpbrew consegue
identificar os locais de instalação da libgd e libpng.

Como de costume, usaremos o brew para ver detalhes da instalação das bibliotecas.

```bash
$ brew info zlib
zlib: stable 1.2.11 (bottled), HEAD [keg-only]
General-purpose lossless data-compression library
https://zlib.net/
/usr/local/Cellar/zlib/1.2.11 (12 files, 376.4KB)
  Poured from bottle on 2020-10-08 at 18:30:00
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/zlib.rb
License: Zlib
==> Options
--HEAD
  Install HEAD version
==> Caveats
zlib is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

For compilers to find zlib you may need to set:
  export LDFLAGS="-L/usr/local/opt/zlib/lib"
  export CPPFLAGS="-I/usr/local/opt/zlib/include"

For pkg-config to find zlib you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/zlib/lib/pkgconfig"

==> Analytics
install: 32,521 (30 days), 95,108 (90 days), 323,711 (365 days)
install-on-request: 32,012 (30 days), 93,392 (90 days), 315,335 (365 days)
build-error: 0 (30 days)
```

```bash
$ brew info jpeg
jpeg: stable 9d (bottled)
Image manipulation library
https://www.ijg.org/
/usr/local/Cellar/jpeg/9d (21 files, 775.2KB) *
  Poured from bottle on 2020-03-19 at 12:37:49
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/jpeg.rb
License: IJG
==> Analytics
install: 139,023 (30 days), 408,612 (90 days), 1,924,789 (365 days)
install-on-request: 5,096 (30 days), 14,926 (90 days), 69,303 (365 days)
build-error: 0 (30 days)
```

Com os locais de instalação identificados, pode-se finalmente instalar a extensão, usando o comando a seguir:

```bash
$ phpbrew ext install gd -- --with-zlib=/usr/local/opt/zlib --with-jpeg=/usr/local/Cellar/jpeg/9d
```

## Intl

De acordo com o [manual](https://www.php.net/manual/en/intro.intl.php) do PHP, Intl é uma extensão que dá acesso à
biblioteca [ICU](http://www.icu-project.org), que é usada para operações de internacionalização (formatação de números,
mensagens, etc).

Antes de mais nada, é necessário instalar o pacote **icu4c**, usando o comando abaixo:

```bash
$ brew install icu4c
```

Mais uma vez, será importante verificar o local de instalação do pacote icu4c, usando o brew conforme abaixo:

```bash
$ brew info icu4c
icu4c: stable 67.1 (bottled) [keg-only]
C/C++ and Java libraries for Unicode and globalization
http://site.icu-project.org/home
/usr/local/Cellar/icu4c/67.1 (258 files, 71.2MB)
  Poured from bottle on 2020-06-03 at 09:55:28
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/icu4c.rb
License: ICU
==> Caveats
icu4c is keg-only, which means it was not symlinked into /usr/local,
because macOS provides libicucore.dylib (but nothing else).

If you need to have icu4c first in your PATH, run:
  echo 'export PATH="/usr/local/opt/icu4c/bin:$PATH"' >> /Users/flavioheleno/.bash_profile
  echo 'export PATH="/usr/local/opt/icu4c/sbin:$PATH"' >> /Users/flavioheleno/.bash_profile

For compilers to find icu4c you may need to set:
  export LDFLAGS="-L/usr/local/opt/icu4c/lib"
  export CPPFLAGS="-I/usr/local/opt/icu4c/include"

For pkg-config to find icu4c you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/icu4c/lib/pkgconfig"

==> Analytics
install: 293,705 (30 days), 921,498 (90 days), 4,549,042 (365 days)
install-on-request: 5,859 (30 days), 18,851 (90 days), 129,734 (365 days)
build-error: 0 (30 days)
```

Com o local de instalação identificado, pode-se finalmente instalar a extensão, usando o comando a seguir:

```bash
PKG_CONFIG_PATH="/usr/local/opt/icu4c/lib/pkgconfig" phpbrew ext install intl
```
