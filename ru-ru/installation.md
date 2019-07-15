* * *

layout: default language: 'en' version: '0.11'

* * *

# Установка

Следуйте инструкциям ниже, чтобы установить Zephir:

<a name='prerequisites'></a>

## Системные требования

Чтобы собрать расширение под PHP и использовать Zephir нужно:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* Один из следующих C компиляторов: [gcc](https://gcc.gnu.org/) >= 4.4, [clang](https://clang.llvm.org/) >= 3.0, [Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) >= 11 или [Intel C++](https://software.intel.com/en-us/c-compilers). Рекомендуется `gcc` 4.4 или старше
* [re2c](http://re2c.org/) 0.13.6 или старше
* Заголовочные файлы PHP и инструменты разработчика

Для систем на базе Linux, вам понадобится также:

* [GNU make](https://www.gnu.org/software/make/) 3.81 или старше
* [autoconf](https://www.gnu.org/software/autoconf/autoconf.html) 2.31 или старше
* [automake](https://www.gnu.org/software/automake/) 1.14 или старше
* libpcre3
* Пакет `build-essential`, если вы используете `gcc` в Ubuntu (и, вероятно, в других дистрибутивах)

В Ubuntu эти пакеты можно поставить так:

```bash
sudo apt-get update
sudo apt-get install git gcc make re2c php php-json php-dev libpcre3-dev build-essential
```

Пожалуйста, обратите внимание, что конкретные версии библиотек и программного обеспечения на момент прочтения этого руководства могут измениться.

Так как Zephir написан на PHP, вам нужно установить последнюю версию PHP. PHP должен быть доступен из консоли:

```bash
php -v
```

    PHP 7.2.17-0ubuntu0.19.04.1 (cli) (built: Apr 18 2019 18:01:25) ( NTS )
    Copyright (c) 1997-2018 The PHP Group
    Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
        with Zend OPcache v7.2.17-0ubuntu0.19.04.1, Copyright (c) 1999-2018, by Zend Technologies
        with Xdebug v2.6.1, Copyright (c) 2002-2018, by Derick Rethans
    

Также проверьте, доступны ли инструменты для сборки расширений:

```bash
phpize -v
```

    Configuring for:
    PHP Api Version:         20170718
    Zend Module Api No:      20170718
    Zend Extension Api No:   320170718
    

Вам не обязательно нужно получить точно такой же вывод. Однако важно, чтобы эти команды были доступны для начала разработки на Zephir.

<a name='installing-zephir'></a>

## Установка Zephir

Для начала убедитесь что Zephir parser установлен и активирован. За подробностями обратитесь к [следующему руководству](https://github.com/phalcon/php-zephir-parser).

### Release PHAR

The recommended, **officially supported**, and easiest-to-use way to install Zephir is to simply grab the latest release PHAR [from GitHub](https://github.com/phalcon/zephir/releases/latest), and download/move it to somewhere in your `$PATH`. (You'll probably also want to rename it to drop the `.phar` extension, so you can run it as `zephir` instead of `zephir.phar`.)

### Composer

The PHAR isn't available before 0.11.4, so if you need an older version, you can use Composer, in one of two ways:

#### Global Composer Application

```bash
composer global require phalcon/zephir
```

There are two approaches to running Zephir at this point. The first is to ensure that `${COMPOSER_HOME}/vendor/bin` is in your `$PATH`, then Zephir should be available as `zephir` on the command line. The second is to simply use `composer global exec zephir` instead.

#### Project Dependency

```bash
composer require phalcon/zephir
```

Use `composer exec zephir` within the project you installed Zephir in, above, to run it. (Alternately, you can still run `vendor/bin/zephir`.)

### Git Clone

Finally, you can also simply clone the latest tag from GitHub, install the dependencies, and run Zephir from there:

```bash
git clone --depth 1 -b $(git ls-remote https://github.com/phalcon/zephir 0.11.* | sort -t/ -k3 -Vr | head -n1 | awk -F/ '{ print $NF }') https://github.com/phalcon/zephir
composer install
```

You'll need to either use the path to `zephir/zephir`, or create a symlink in a directory in your `$PATH`, to run Zephir using this option.

<a name='testing-the-installation'></a>

## Протестируйте Zephir

Проверьте, доступен ли Zephir из любой директории следующей командой:

```bash
zephir list
```