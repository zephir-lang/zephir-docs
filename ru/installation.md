# Installation

To install Zephir, please follow these steps:

<a name='prerequisites'></a>

## Prerequisites

To build a PHP extension and use Zephir you need the following requirements:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* gcc >= 4.x/clang >= 3.x
* re2c 0.13 or later
* gnu make 3.81 or later
* autoconf 2.31 or later
* automake 1.14 or later
* libpcre3
* PHP development headers and tools
* The build-essential package when using gcc on Ubuntu (and likely in other distributions as well)

If you're using Ubuntu, you can install the required packages this way:

    $ sudo apt-get update
    $ sudo apt-get install git gcc make re2c php php-json php-dev libpcre3-dev build-essential
    

Since Zephir is written in PHP, you need to have a recent version of PHP installed, and it must be available in your console:

    $ php -v
    PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
    Copyright (c) 1997-2016 The PHP Group
    Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
            with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies
    

Also, make sure you have the PHP development libraries installed along with your PHP installation:

    $ phpize -v
    Configuring for:
    PHP Api Version:         20151012
    Zend Module Api No:      20151012
    Zend Extension Api No:   320151012
    

You don't have to necessarily see the exact above output, but it's important that these commands are available to start developing with Zephir.

<a name='installing-zephir'></a>

## Installing Zephir

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