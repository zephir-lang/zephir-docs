安装
============
要安装Zephir扩展，请按照如下步骤进行：

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
	$ sudo apt-get install git gcc make re2c php7.0 php7.0-json php7.0-dev libpcre3-dev

因为Zephir是用PHP编写的，所以你需要安装最新版本的PHP和控制台:

.. code-block:: bash

	$ php -v
	PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
	Copyright (c) 1997-2016 The PHP Group
	Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
    		with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies

同时,确保你也一起安装了PHP开发库:

.. code-block:: bash

	$ phpize -v
	Configuring for:
	PHP Api Version:         20151012
	Zend Module Api No:      20151012
	Zend Extension Api No:   320151012

查看如上信息不是必须的，重要的是phpize是可用的，且php是可用的即可，因为要使用zephir进行开发你必须配置php-cli的运行环境。

安装 Zephir
-----------------

Zephir目前必须从Github克隆:

.. code-block:: bash

	$ git clone https://github.com/phalcon/zephir

执行Zephir安装 (this compiles/creates the parser):

.. code-block:: bash

	$ cd zephir
	$ ./install -c

检查安装
--------------------
要检查zephir的安装，你可以在任意目录下执行:

.. code-block:: bash

	$ zephir help
