# Встановлення

Щоб встановити Zephir, будь ласка, слідуйте наступним крокам:

<a name='prerequisites'></a>

## Передумови

Щоб створити PHP-розширення за допомогою Zephir, вам потрібні наступні програми та засоби:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* Один з наступних C компіляторів: [gcc](https://gcc.gnu.org/) >= 4.4, [clang](https://clang.llvm.org/) >= 3.0, [Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) >= 11 або[Intel C++](https://software.intel.com/en-us/c-compilers). Рекомендується `gcc` 4.4 або старше
* [re2c](http://re2c.org/) 0.13.6 або старше
* Заголовки та інструменти розробника PHP

Для систем на базі Linux, вам також знадобиться:

* [GNU make](https://www.gnu.org/software/make/) 3.81 або старше
* [autoconf](https://www.gnu.org/software/autoconf/autoconf.html) 2.31 або старше
* [automake](https://www.gnu.org/software/automake/) 1.14 або старше
* libpcre3
* The `build-essential` package when using `gcc` on Ubuntu (and likely in other distributions as well)

If you're using Ubuntu, you can install the required packages this way:

```bash
sudo apt-get update
sudo apt-get install git gcc make re2c php php-json php-dev libpcre3-dev build-essential
```

Please note that specific versions of libraries and programs at the time of reading this guide may vary.

Since Zephir is written in PHP, you need to have a recent version of PHP installed, and it must be available in your console:

```bash
php -v
PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
        with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies
```

Also, make sure you have the PHP development libraries installed along with your PHP installation:

```bash
phpize -v
Configuring for:
PHP Api Version:         20151012
Zend Module Api No:      20151012
Zend Extension Api No:   320151012
```

You don't have to necessarily see the exact above output, but it's important that these commands are available to start developing with Zephir.

<a name='installing-zephir'></a>

## Встановлення Zephir

<a name='git-way'></a>

### Git Way

First make sure that the Zephir parser extension is installed and activated. You can follow this [tutorial](https://github.com/phalcon/php-zephir-parser).

The Zephir compiler currently must be cloned from Github:

```bash
git clone https://github.com/phalcon/zephir
```

Run the Zephir installer:

```bash
cd zephir
./install -c
```

The last thing you need is to make sure you have all the necessary dependencies and install additional PHP libraries:

```bash
composer install
```

This step is optional for version 0.10.x, however, it will become mandatory in future versions.

<a name='testing-the-installation'></a>

## Тестування встановлення

Check if Zephir is available from any directory by executing:

```bash
zephir help
```