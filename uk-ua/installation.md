---
layout: default
language: 'uk-ua'
version: '0.11'
---

# Встановлення

Щоб встановити Zephir, будь ласка, слідуйте наступним крокам:

<a name='prerequisites'></a>

## Передумови

Щоб створити PHP-розширення за допомогою Zephir, вам потрібні наступні програми та засоби:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* Один з наступних C компіляторів: [gcc](https://gcc.gnu.org/) >= 4.4, [clang](https://clang.llvm.org/) >= 3.0, [Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) >= 11 або[Intel C++](https://software.intel.com/en-us/c-compilers). Рекомендується `gcc` 4.4 або старше
* [re2c](http://re2c.org/) 0.13.6 або старше
* Заголовки та інструменти розробника PHP

Для систем на базі Linux, вам також знадобиться:

* [GNU make](https://www.gnu.org/software/make/) 3.81 або старше
* [autoconf](https://www.gnu.org/software/autoconf/autoconf.html) 2.31 або старше
* [automake](https://www.gnu.org/software/automake/) 1.14 або старше
* libpcre3
* Пакет `build-essential`, якщо ви використовуєте `gcc` в Ubuntu (і, ймовірно, в інших дистрибутивах)

Якщо ви використовуєте Ubuntu, то можете встановити необхідні пакунки таким чином:

```bash
sudo apt-get update
sudo apt-get install git gcc make re2c php php-json php-dev libpcre3-dev build-essential
```

Будь ласка, зверніть увагу, що конкретні версії бібліотек і програмного забезпечення на момент прочитання цього посібника можуть змінитися.

Оскільки Zephir написаний на PHP, вам необхідна остання версія PHP, яка буде доступна через консоль:

```bash
php -v
```
```
PHP 7.2.17-0ubuntu0.19.04.1 (cli) (built: Apr 18 2019 18:01:25) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.17-0ubuntu0.19.04.1, Copyright (c) 1999-2018, by Zend Technologies
    with Xdebug v2.6.1, Copyright (c) 2002-2018, by Derick Rethans
```

Також переконайтеся, що у вас встановлені пакунки бібліотек для розробки PHP:

```bash
phpize -v
```
```
Configuring for:
PHP Api Version:         20170718
Zend Module Api No:      20170718
Zend Extension Api No:   320170718
```

Вам не обов'язково потрібно отримати такий самий вивід. Однак важливо, щоб ці команди були доступні для початку розробки на Zephir.

<a name='installing-zephir'></a>

## Встановлення Zephir

Для початку переконайтеся, що Zephir parser встановлений і активований. За подробицями зверніться до [наступного посібника](https://github.com/phalcon/php-zephir-parser).

### Використовуючи PHAR

Рекомендований, **з офіційною підтримкою**, і найпростіший спосіб встановити Zephir це просто завантажити останню версію PHAR з [GitHub-а](https://github.com/phalcon/zephir/releases/latest), і вказати до нього шлях в змінній оточення `$PATH`. Ви, ймовірно, захочете позбутися від розширення `.phar`, щоб запускати його як `zephir`, а не `zephir.phar`.

### Використовуючи Composer

PHAR не доступний до версії 0.11.4, тому якщо вам потрібна старіша версія, ви можете використовувати Composer одним з двох способів:

#### Як глобальну Composer програму

```bash
composer global require phalcon/zephir
```

На даний момент існує два підходи до запуску Zephir. Перший, переконайтеся, що `${COMPOSER_HOME}/vendor/bin` знаходиться у вашому `$PATH`, Zephir повинен бути доступний в командному рядку за допомогою команди `zephir`. Другий, замість цього можна використовувати команду `composer global exec zephir`.

#### Як локальна залежність в проекті

```bash
composer require phalcon/zephir
```

Для запуску Zephir використовуйте команду `composer exec zephir` в проекті з встановленим Zephir як показано вище. (Крім того, як альтернативу ви можете запустити `vendor/bin/zephir`

### Використовуючи Git

На останок, ви можете просто склонувати останні теги з GitHub, встановити залежності й запустити Zephir:

```bash
git clone --depth 1 -b $(git ls-remote https://github.com/phalcon/zephir 0.11.* | sort -t/ -k3 -Vr | head -n1 | awk -F/ '{ print $NF }') https://github.com/phalcon/zephir
composer install
```

Для запуску Zephir з використанням цього варіанту вам потрібно або використовувати шлях до `zephir/zephir`, або створити символьне посилання в каталозі `$PATH`.

<a name='testing-the-installation'></a>

## Тестування встановлення

Перевірте, чи доступний Zephir з будь-якого каталогу за допомогою такої команди:

```bash
zephir list
```
