* * *

layout: default language: 'en' version: '0.11'

* * *

# Установка

Следуйте инструкциям ниже, чтобы установить Zephir:

<a name='prerequisites'></a>

## Системные требования

Чтобы собрать расширение под PHP и использовать Zephir нужно:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* Один из следующих C компиляторов: [gcc](https://gcc.gnu.org/) >= 4.4, [clang](https://clang.llvm.org/) >= 3.0, [Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) >= 11 или [Intel C++](https://software.intel.com/en-us/c-compilers). Рекомендуется `gcc` 4.4 или старше
* [re2c](http://re2c.org/) 0.13.6 или старше
* Заголовочные файлы PHP и инструменты разработчика

Для систем на базе Linux, вам понадобится также:

* [GNU make](https://www.gnu.org/software/make/) 3.81 или старше
* [autoconf](https://www.gnu.org/software/autoconf/autoconf.html) 2.31 или старше
* [automake](https://www.gnu.org/software/automake/) 1.14 или старше
* libpcre3
* Пакет `build-essential`, если вы используете `gcc` в Ubuntu (и, вероятно, в других дистрибутивах)

В Ubuntu эти пакеты можно поставить так:

```bash
sudo apt-get update
sudo apt-get install git gcc make re2c php php-json php-dev libpcre3-dev build-essential
```

Пожалуйста, обратите внимание, что конкретные версии библиотек и программного обеспечения на момент прочтения этого руководства могут измениться.

Так как Zephir написан на PHP, вам нужно установить последнюю версию PHP. PHP должен быть доступен из консоли:

```bash
php -v
PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
        with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies
```

Также проверьте, доступны ли инструменты для сборки расширений:

```bash
phpize -v
Configuring for:
PHP Api Version:         20151012
Zend Module Api No:      20151012
Zend Extension Api No:   320151012
```

Вам не обязательно нужно получить точно такой же вывод. Однако важно, чтобы эти команды были доступны для начала разработки на Zephir.

<a name='installing-zephir'></a>

## Установка Zephir

<a name='git-way'></a>

### С использованием Git

Для начала убедитесь что Zephir parser установлен и активирован. За подробностями обратитесь к [следующему руководству](https://github.com/phalcon/php-zephir-parser).

Склонируйте репозиторий Zephir с Github:

```bash
git clone https://github.com/phalcon/zephir
```

Запустите инсталятор:

```bash
cd zephir
./install -c
```

Последнее, что вам необходимо сделать, это убедиться, что у вас включены все необходимые расширения и установлены все зависимости:

```bash
composer install
```

Этот шаг не является обязательным для версии 0.10.x, однако он станет обязательным для последующих версий.

<a name='testing-the-installation'></a>

## Протестируйте Zephir

Проверьте, доступен ли Zephir из любой директории следующей командой:

```bash
zephir help
```