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
	$ sudo apt-get install git gcc make re2c php5 php5-json php5-dev libpcre3-dev

Так как Zephir написан на PHP, вам нужно установить последнюю версию PHP.
PHP должен быть доступен из консоли:

.. code-block:: bash

	$ php -v
	PHP 5.5.7 (cli) (built: Dec 14 2013 00:44:43)
	Copyright (c) 1997-2013 The PHP Group
	Zend Engine v2.5.0, Copyright (c) 1998-2013 Zend Technologies

Также проверьте, доступны ли dev-инструменты для сборки расширений:

.. code-block:: bash

	$ phpize -v
	Configuring for:
	PHP Api Version:         20121113
	Zend Module Api No:      20121212
	Zend Extension Api No:   220121212


Установка Zephir
----------------

Склонируйте репозиторий Zephir:

.. code-block:: bash

	$ git clone https://github.com/phalcon/zephir
	$ cd zephir

Json-C необходим для работы парсера:

.. code-block:: bash

	$ cd json-c
	$ sh autogen.sh
	$ ./configure
	$ make && sudo make install


Чтобы скомпилировать и установить Zephir выполните следующие команды:

.. code-block:: bash

	$ cd zephir
	$ ./install -c

Протестируйте Zephir
--------------------
Проверьте, доступен ли Zephir из любой директории командой:

.. code-block:: bash

	$ zephir help
