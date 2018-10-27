# Установка

Следуйте инструкциям ниже, чтобы установить Zephir:

<a name='prerequisites'></a>

## Системные требования

Чтобы собрать расширение под PHP и использовать Zephir нужно:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* gcc >= 4.x/clang >= 3.x
* re2c 0.13 или более поздней версии
* gnu make 3.81 или более поздней версии
* autoconf 2.31 или более поздней версии
* automake 1.14 или более поздней версии
* libpcre3
* Заголовочные файлы PHP и инструменты разработчика
* Пакет build-essential если вы используете gcc на Ubuntu (и, вероятно, в других дистрибутивах)

На Ubuntu эти пакеты можно поставить так:

    $ sudo apt-get update
    $ sudo apt-get install git gcc make re2c php php-json php-dev libpcre3-dev build-essential
    

Так как Zephir написан на PHP, вам нужно установить последнюю версию PHP. PHP должен быть доступен из консоли:

    $ php -v
    PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
    Copyright (c) 1997-2016 The PHP Group
    Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
            with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies
    

Также проверьте, доступны ли инструменты для сборки расширений:

    $ phpize -v
    Configuring for:
    PHP Api Version:         20151012
    Zend Module Api No:      20151012
    Zend Extension Api No:   320151012
    

Вам не обязательно нужно получить точно такой же вывод. Однако важно, чтобы эти команды были доступны для начала разработки на Zephir.

<a name='installing-zephir'></a>

## Установка Zephir

Во-первых, убедитесь, что расширение Zephir parser установлено и активировано, в соответствии с [этим руководством](https://github.com/phalcon/php-zephir-parser).

Склонируйте репозиторий Zephir с Github:

    $ git clone https://github.com/phalcon/zephir
    

Запустите установщик:

    $ cd zephir
    $ ./install -c
    

<a name='testing-the-installation'></a>

## Протестируйте Zephir

Проверьте, доступен ли Zephir из любой директории командой:

    $ zephir help