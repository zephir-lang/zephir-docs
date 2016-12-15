Установка
============

Системные требования
--------------------

Чтобы собрать расширение под PHP и использовать Zephir нужно:

* gcc >= 4.x/clang >= 3.x
* re2c 0.13 or later
* gnu make >=3.81
* autoconf >=2.31
* automake >=1.14
* libpcre3
* php development headers and tools

На Ubuntu эти пакеты можно поставить так:

.. code-block:: bash

	$ sudo apt-get update
	$ sudo apt-get install git gcc make re2c php7.0 php7.0-json php7.0-dev libpcre3-dev

Так как Zephir написан на PHP, вам нужно установить последнюю версию PHP.
PHP должен быть доступен из консоли:

.. code-block:: bash

	$ php -v
	PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
	Copyright (c) 1997-2016 The PHP Group
	Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
    		with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies

Также проверьте, доступны ли dev-инструменты для сборки расширений:

.. code-block:: bash

	$ phpize -v
	Configuring for:
	PHP Api Version:         20151012
	Zend Module Api No:      20151012
	Zend Extension Api No:   320151012


Установка Zephir
----------------

Склонируйте репозиторий Zephir:

.. code-block:: bash

	$ git clone https://github.com/phalcon/zephir
	$ cd zephir

Чтобы скомпилировать и установить Zephir выполните следующие команды:

.. code-block:: bash

	$ cd zephir
	$ ./install -c

Протестируйте Zephir
--------------------
Проверьте, доступен ли Zephir из любой директории командой:

.. code-block:: bash

	$ zephir help
