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
	$ sudo apt-get install git gcc make re2c php5 php5-json php5-dev libpcre3-dev

Since Zephir is written in PHP you need to have installed a recent version of PHP and it must be available in your console:

.. code-block:: bash

	$ php -v
	PHP 5.5.7 (cli) (built: Dec 14 2013 00:44:43)
	Copyright (c) 1997-2013 The PHP Group
	Zend Engine v2.5.0, Copyright (c) 1998-2013 Zend Technologies

Also, make sure you have also the PHP development libraries installed along with your PHP installation:

.. code-block:: bash

	$ phpize -v
	Configuring for:
	PHP Api Version:         20121113
	Zend Module Api No:      20121212
	Zend Extension Api No:   220121212

You don't have to necessarely see the exact above output but it's important that these commands are available to start
developing with Zephir.

Installing Zephir
-----------------
Json-C is required by the Zephir parser to process the code, you can install it this way:

.. code-block:: bash

	$ git submodule update --init
	$ cd json-c
	$ sh autogen.sh
	$ ./configure
	$ make && sudo make install

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
