---
layout: default
language: 'en'
version: '0.11'
---

# Installation

To install Zephir, please follow these steps:

<a name='prerequisites'></a>

## Prerequisites

To build a PHP extension and use Zephir you need the following requirements:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* A C compiler such as [gcc](https://gcc.gnu.org/) >= 4.4 or an alternative such as [clang](https://clang.llvm.org/) >= 3.0, [Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) >= 11 or [Intel C++](https://software.intel.com/en-us/c-compilers). It is recommended to use `gcc` 4.4 or later
* [re2c](http://re2c.org/) 0.13.6 or later
* PHP development headers and tools

For Linux based systems you'll need also:

* [GNU make](https://www.gnu.org/software/make/) 3.81 or later
* [autoconf](https://www.gnu.org/software/autoconf/autoconf.html) 2.31 or later
* [automake](https://www.gnu.org/software/automake/) 1.14 or later
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
```

    PHP 7.2.17-0ubuntu0.19.04.1 (cli) (built: Apr 18 2019 18:01:25) ( NTS )
    Copyright (c) 1997-2018 The PHP Group
    Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
        with Zend OPcache v7.2.17-0ubuntu0.19.04.1, Copyright (c) 1999-2018, by Zend Technologies
        with Xdebug v2.6.1, Copyright (c) 2002-2018, by Derick Rethans
    

Also, make sure you have the PHP development libraries installed along with your PHP installation:

```bash
phpize -v
```

    Configuring for:
    PHP Api Version:         20170718
    Zend Module Api No:      20170718
    Zend Extension Api No:   320170718
    

You don't have to necessarily see the exact above output, but it's important that these commands are available to start developing with Zephir.

<a name='installing-zephir'></a>

## Installing Zephir

First make sure that the Zephir parser extension is installed and activated. You can follow this [tutorial](https://github.com/phalcon/php-zephir-parser).

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

## Testing the Installation

Check if Zephir is available from any directory by executing:

```bash
zephir list
```
