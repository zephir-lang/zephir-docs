# Встановлення

Щоб встановити Zephir, будь ласка, слідуйте наступним крокам:

<a name='prerequisites'></a>

## Передумови

Щоб створити PHP-розширення за допомогою Zephir, вам потрібні наступні програми та засоби:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* gcc >= 4.x/clang >= 3.x
* re2c 0.13 або новіший
* gnu make 3.81 або новіший
* autoconf 2.31 або новіший
* automake 1.14 або новіший
* libpcre3
* php development headers and tools
* The build-essential package when using gcc on Ubuntu (and likely other distributions as well)

Якщо ви використовуєте Ubuntu, то можете встановити необхідні пакунки таким чином:

    $ sudo apt-get update
    $ sudo apt-get install git gcc make re2c php php-json php-dev libpcre3-dev build-essential
    

Оскільки Zephir написаний на PHP, вам необхідна остання версія PHP, яка буде доступна через консоль:

    $ php -v
    PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
    Copyright (c) 1997-2016 The PHP Group
    Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
            with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies
    

Також переконайтеся, що у вас встановлені пакунки бібліотек для розробки PHP:

    $ phpize -v
    Configuring for:
    PHP Api Version:         20151012
    Zend Module Api No:      20151012
    Zend Extension Api No:   320151012
    

Наведені вище числа - лише для прикладу, у вас вони можуть відрізнятися. Важливо, щоб ці команди працювали.

<a name='installing-zephir'></a>

## Встановлення Zephir

First, make sure the Zephir parser extension is installed and activated following its [tutorial](https://github.com/phalcon/php-zephir-parser)

The Zephir compiler currently must be cloned from Github:

    $ git clone https://github.com/phalcon/zephir
    

Run the Zephir installer (this compiles/creates the parser):

    $ cd zephir
    $ ./install -c
    

<a name='testing-the-installation'></a>

## Testing the Installation

Check if Zephir is available from any directory by executing:

    $ zephir help