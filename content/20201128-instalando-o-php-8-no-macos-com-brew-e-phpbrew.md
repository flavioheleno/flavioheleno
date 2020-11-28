# Instalando o PHP 8 no macOS Catalina com brew e phpbrew

Neste texto, será abordada a instalação no macOS Catalina do recém lançado
[PHP 8](https://www.php.net/archive/2020.php#2020-11-26-3) usando [brew](https://brew.sh/) e
[phpbrew](https://github.com/phpbrew/phpbrew).

O **brew** será usado para instalar as dependências necessárias para compilar o PHP e o **phpbrew** será usado para de
fato compilar e instalar o PHP e algumas extensões.

Vale notar que nem todas as extensões já suportam a nova versão do PHP, conforme levantado pelo
[Remi Collet](https://twitter.com/RemiCollet) no artigo _[PHP extensions status with upcoming PHP
8.0](https://blog.remirepo.net/post/2020/09/21/PHP-extensions-status-with-upcoming-PHP-8.0)_.

## Dependências

As dependências para compilação podem ser instaladas executando o comando abaixo.

```bash
$ brew install autoconf bison re2c pkg-config libxml2 bzip2 libzip oniguruma mhash curl
```

É bem possível que o erro abaixo, referente a versão do **bison**, seja exibido na tela:

```bash
checking for bison... bison

checking for bison version... 2.3

configure: error: bison 3.0.0 or later is required to generate PHP parsers (excluded versions: none).
```

Este erro é causado pela existência de duas versões do bison disponíveis no MacOS:

1. A versão distribuída com o MacOS:
```bash
$ bison --version
bison (GNU Bison) 2.3
Written by Robert Corbett and Richard Stallman.

Copyright (C) 2006 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

$ which bison
/usr/bin/bison
```

2. A versão instalada anteriormente com o brew:
```bash
$ brew info bison
bison: stable 3.7.2 (bottled) [keg-only]
Parser generator
https://www.gnu.org/software/bison/
/usr/local/Cellar/bison/3.7.2 (94 files, 3.3MB)
  Poured from bottle on 2020-10-02 at 17:32:30
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/bison.rb
License: GPL-3.0-or-later
==> Caveats
bison is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have bison first in your PATH run:
  echo 'export PATH="/usr/local/opt/bison/bin:$PATH"' >> /Users/flavioheleno/.bash_profile

For compilers to find bison you may need to set:
  export LDFLAGS="-L/usr/local/opt/bison/lib"

==> Analytics
install: 19,009 (30 days), 57,914 (90 days), 236,707 (365 days)
install-on-request: 12,035 (30 days), 34,591 (90 days), 147,094 (365 days)
build-error: 0 (30 days)
```

Para garantir que o phpbrew use a versão correta do bison, basta alterar a variável `PATH` conforme o comando abaixo,
apenas para a execução da compilação e instalação.

```bash
$ PATH="/usr/local/opt/bison/bin:$PATH" phpbrew install ...
```

## Compilando e Instalando

A compilação e instalação da última versão disponível pode ser feita executando o comando abaixo.

```bash
$ phpbrew install php-8.0.0
```

Obs: para compilar e instalar a versão de desenvolvimento (da branch master), basta executar, alternativamente,  o
comando abaixo.

```bash
$ phpbrew install next as php-8.0dev
```

Ao término da compilação/instalação, a mensagem abaixo será apresentada na tela do terminal:

```bash
Congratulations! Now you have PHP with php-8.0.0 as php-8.0.0

* To configure your installed PHP further, you can edit the config file at
    /Users/flavioheleno/.phpbrew/php/php-8.0.0/etc/php.ini

To use the newly built PHP, try the line(s) below:

    $ phpbrew use php-8.0.0

Or you can use switch command to switch your default php to php-8.0.0:

    $ phpbrew switch php-8.0.0

Enjoy!
```

Para alternar para a nova versão, basta executar o comando abaixo.

```bash
$ phpbrew use php-8.0.0
```

Note que ao abrir uma nova janela do terminal, a versão original do PHP distribuída com o MacOS será usada e não mais a
versão recentemente instalada. É possível trocar de maneira definitiva a versão usada pelo terminal executando o
comando abaixo.

```bash
$ phpbrew switch php-8.0.0
```

Mais informações sobre o phpbrew podem ser encontradas na [documentação oficial](https://github.com/phpbrew/phpbrew).

## Extensões

Pode-se verificar quais extensões estão instaladas e quais estão disponíveis para instalação, executando o comando
abaixo.

```bash
$ phpbrew ext
```

Para instalar uma nova extensão, pode-se usar algumas alternativas, conforme demonstrado abaixo.

1. Extensões disponíveis via pecl

Para instalar a extensão [pdo_pgsql](https://pecl.php.net/package/PDO_PGSQL), é necessário primeiramente instalar as
suas dependências, assim como feito com o PHP este passo é realizado usando o brew, conforme o comando abaixo.

```bash
$ brew install libpq
```

Ao tentar instalar a extensão após a instalação da **libpq** é bem provável que o comando falhe e ao investigar a causa
no arquivo de log nota-se o erro abaixo.

```bash
...
checking for PostgreSQL support for PDO... yes, shared
checking for pg_config... not found
configure: error: Cannot find libpq-fe.h. Please specify correct PostgreSQL installation path
```

Este erro tem sua causa no processo de compilação em que o compilador não consegue encontrar o arquivo **libpq-fe.h**.

Para identificar o caminho no qual a libpq foi instalada, pode-se recorrer ao comando abaixo.

```bash
$ brew info libpq
libpq: stable 13.0 (bottled) [keg-only]
Postgres C API library
https://www.postgresql.org/docs/12/libpq.html
/usr/local/Cellar/libpq/13.0 (2,268 files, 25.4MB)
  Poured from bottle on 2020-10-02 at 21:16:20
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/libpq.rb
License: PostgreSQL
==> Dependencies
Required: krb5 ✔, openssl@1.1 ✔
==> Caveats
libpq is keg-only, which means it was not symlinked into /usr/local,
because conflicts with postgres formula.

If you need to have libpq first in your PATH run:
  echo 'export PATH="/usr/local/opt/libpq/bin:$PATH"' >> /Users/flavioheleno/.bash_profile

For compilers to find libpq you may need to set:
  export LDFLAGS="-L/usr/local/opt/libpq/lib"
  export CPPFLAGS="-I/usr/local/opt/libpq/include"

For pkg-config to find libpq you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/libpq/lib/pkgconfig"

==> Analytics
install: 87,113 (30 days), 237,461 (90 days), 844,343 (365 days)
install-on-request: 20,308 (30 days), 58,107 (90 days), 128,767 (365 days)
build-error: 0 (30 days)

```

Desta vez, podemos usar o parâmetro `--with-pdo-pgsql` para indicar onde a
**libpq** está instalada, conforme o comando abaixo.

```bash
$ phpbrew ext install pdo_pgsql -- --with-pdo-pgsql=/usr/local/opt/libpq
```


2. Extensões disponíveis via github

Mais informações sobre o comando `phpbrew ext` podem ser encontradas na
[documentação oficial](https://github.com/phpbrew/phpbrew/wiki/Extension-Installer).
