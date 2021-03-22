---
layout: default
language: 'uk-ua'
version: '0.12'
---

# Встановлення

Щоб встановити Zephir, будь ласка, слідуйте наступним крокам:

<a id='prerequisites'></a>

## Передумови

Щоб створити PHP-розширення за допомогою Zephir, вам потрібні наступні програми та засоби:

* [Zephir parser](https://github.com/zephir-lang/php-zephir-parser) >= 1.3.0
* Один з наступних C компіляторів: [gcc](https://gcc.gnu.org/) >= 4.4, [clang](https://clang.llvm.org/) >= 3.0, [Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) >= 11 або[Intel C++](https://software.intel.com/en-us/c-compilers). Рекомендується `gcc` 4.4 або старше
* [re2c](https://re2c.org/) 0.13.6 or later
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
PHP 7.3.7 (cli) (built: Jul 14 2019 17:24:22) ( ZTS DEBUG )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.7, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.3.7, Copyright (c) 1999-2018, by Zend Technologies
    with Xdebug v2.7.2, Copyright (c) 2002-2019, by Derick Rethans
```

Також переконайтеся, що у вас встановлені пакунки бібліотек для розробки PHP:

```bash
phpize -v
Configuring for:
PHP Api Version:         20180731
Zend Module Api No:      20180731
Zend Extension Api No:   320180731
```

Вам не обов'язково потрібно отримати такий самий вивід. Однак важливо, щоб ці команди були доступні для початку розробки на Zephir.

<a id='installing-zephir'></a>

## Встановлення Zephir

Для початку переконайтеся, що Zephir parser встановлений і активований. You can find installation instructions in the [Zephir Parser repository](https://github.com/zephir-lang/php-zephir-parser).

### Реліз PHAR

The recommended, **officially supported**, and easiest-to-use way to install Zephir is to simply grab the latest release PHAR [from GitHub](https://github.com/zephir-lang/zephir/releases/latest), and download/move it to somewhere in your `$PATH`. (Ви, ймовірно, також захочете позбутися від розширення `.phar`, щоб запускати його як `zephir`, а не `zephir.phar`.)

### Composer

PHAR не доступний до версії 0.11.4, тому якщо вам потрібна старіша версія, ви можете використовувати Composer одним з двох способів:

#### Composer як глобальна програма

```bash
composer global require zephir-lang/zephir
```

На даний момент існує два підходи до запуску Zephir. Перший, переконайтеся, що `${COMPOSER_HOME}/vendor/bin` знаходиться у вашому `$PATH`, Zephir повинен бути доступний в командному рядку за допомогою команди `zephir`. Другий, замість цього можна використовувати команду `composer global exec zephir`.

#### Залежність проекту

```bash
composer require zephir-lang/zephir
```

Для запуску Zephir використовуйте команду `composer exec zephir` в проекті з встановленим Zephir як показано вище. (Крім того, як альтернативу ви можете запустити `vendor/bin/zephir`

### Git Clone

На останок, ви можете просто склонувати останні теги з GitHub, встановити залежності й запустити Zephir:

```bash
git clone --depth 1 -b $(git ls-remote https://github.com/zephir-lang/zephir 0.12.* | sort -t/ -k3 -Vr | head -n1 | awk -F/ '{ print $NF }') https://github.com/zephir-lang/zephir
composer install
```

Для запуску Zephir з використанням цього варіанту вам потрібно або використовувати шлях до `zephir/zephir`, або створити символьне посилання в каталозі `$PATH`.

<a id='testing-the-installation'></a>

## Тестування встановлення

Перевірте, чи доступний Zephir з будь-якого каталогу за допомогою такої команди:

```bash
zephir help
```