Installation
============
To install Zephir, please follow these steps:

Prerequisites
-------------

To build a PHP extension and use Zephir you need the following requirements:

* gcc >= 4.x/clang >= 3.x
* re2c 0.13 or later
* gnu make 3.81 or later
* autoconf 2.31 or later
* automake 1.14 or later
* libpcre3
* php development headers and tools

If you're using Ubuntu, you can install the required packages this way:

.. code-block:: bash

	$ sudo apt-get update
	$ sudo apt-get install git gcc make re2c php7.0 php7.0-json php7.0-dev libpcre3-dev

Since Zephir is written in PHP you need to have installed a recent version of PHP and it must be available in your console:

.. code-block:: bash

	$ php -v
	PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
	Copyright (c) 1997-2016 The PHP Group
	Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
    		with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies

Also, make sure you have also the PHP development libraries installed along with your PHP installation:

.. code-block:: bash

	$ phpize -v
	Configuring for:
	PHP Api Version:         20151012
	Zend Module Api No:      20151012
	Zend Extension Api No:   320151012

You don't have to necessarely see the exact above output but it's important that these commands are available to start
developing with Zephir.

Installing Zephir
-----------------

The Zephir compiler currently must be cloned from Github:

.. code-block:: bash

	$ git clone https://github.com/phalcon/zephir

Run the Zephir installer (this compiles/creates the parser):

.. code-block:: bash

	$ cd zephir
	$ ./install -c

Testing Installation
--------------------
Check if Zephir is available from any directory by executing:

.. code-block:: bash

	$ zephir help
