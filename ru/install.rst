Установка
============
Для установки Zephir, выполните следующие действия:

Предпосылки
-------------

Чтобы скомпелировать расширение PHP и использовать Zephir вам нужно следующее:

* gcc >= 4.x/clang >= 3.x
* re2c 0.13 or later
* gnu make 3.81 or later
* autoconf 2.31 or later
* automake 1.14 or later
* libpcre3
* php development headers and tools

Если вы используете Ubuntu, вы можете установить необходимые пакеты следующим образом:

.. code-block:: bash

	$ sudo apt-get update
	$ sudo apt-get install git gcc make re2c php5 php5-dev libpcre3-dev

Поскольку Zephir написан для PHP необходимо установить последнюю версию PHP и она должна быть доступна в консоли:

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

Вы не должны обязательно увидеть такой вывод, но важно, что эти команды доступны для начала разработки с Zephir.

Установка Zephir
-----------------
JSON-C обязателен для Zephir парсера, вы можете установить это следующим образом:

.. code-block:: bash

	$ git clone https://github.com/json-c/json-c.git
	$ cd json-c
	$ sh autogen.sh
	$ ./configure
	$ make && sudo make install

Компилятор Zephir в настоящее время должен быть клонирован из Github:

.. code-block:: bash

	$ git clone https://github.com/phalcon/zephir

Запустите Zephir инсталлятор (это компилирует / создает парсер):

.. code-block:: bash

	$ cd zephir
	$ ./install -c

Проверка установки
--------------------
Проверьте, доступен ли Zephir из любого каталога, выполнив:

.. code-block:: bash

	$ zephir help
