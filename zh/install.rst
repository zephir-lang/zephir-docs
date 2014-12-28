安装
============
To install Zephir, please follow these steps:

先决条件
-------------

建立一个PHP扩展和使用Zephir需要以下要求:

* gcc >= 4.x/clang >= 3.x
* re2c 0.13 or later
* gnu make 3.81 or later
* autoconf 2.31 or later
* automake 1.14 or later
* libpcre3
* php development headers and tools

如果你使用 Ubuntu， 你可以通过以下方式安装所需要的包:

.. code-block:: bash

	$ sudo apt-get update
	$ sudo apt-get install git gcc make re2c php5 php5-json php5-dev libpcre3-dev

因为Zephir是用PHP编写的，所以你需要安装最新版本的PHP和控制台:

.. code-block:: bash

	$ php -v
	PHP 5.5.7 (cli) (built: Dec 14 2013 00:44:43)
	Copyright (c) 1997-2013 The PHP Group
	Zend Engine v2.5.0, Copyright (c) 1998-2013 Zend Technologies

同时,确保你也一起安装了PHP开发库:

.. code-block:: bash

	$ phpize -v
	Configuring for:
	PHP Api Version:         20121113
	Zend Module Api No:      20121212
	Zend Extension Api No:   220121212

You don't have to necessarely see the exact above output but it's important that these commands are available to start
developing with Zephir.

安装 Zephir
-----------------

The Zephir compiler currently must be cloned from Github:

.. code-block:: bash

	$ git clone https://github.com/phalcon/zephir

Run the Zephir installer (this compiles/creates the parser):

.. code-block:: bash

	$ cd zephir
	$ ./install-json
	$ ./install -c

You can ommit the ./install-json step if you already have json-c installed

检查 Installation
--------------------
Check if Zephir is available from any directory by executing:

.. code-block:: bash

	$ zephir help
