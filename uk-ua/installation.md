---
layout: default
language: 'en'
version: '0.12'
---

# Встановлення

Щоб встановити Zephir, будь ласка, слідуйте наступним крокам:

<a name='prerequisites'></a>

## Передумови

Щоб створити PHP-розширення за допомогою Zephir, вам потрібні наступні програми та засоби:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.3.0
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

<a name='installing-zephir'></a>

## Встановлення Zephir

<a name='git-way'></a>

### З використанням Git

Для початку переконайтеся, що Zephir parser встановлений і активований. За подробицями зверніться до [наступного посібника](https://github.com/phalcon/php-zephir-parser).

Склонуюйте репозиторій Zephir з Github:

```bash
git clone https://github.com/phalcon/zephir
```

Запустіть інсталятор Zephir:

```bash
cd zephir
./install -c
```

Останнє, що вам необхідно зробити - це переконатися, що у вас увімкнені всі необхідні розширення та встановлені всі залежності:

```bash
composer install
```

Цей крок не є обов'язковим для версії 0.10.x, проте він стане обов'язковим для наступних версій.

<a name='testing-the-installation'></a>

## Тестування встановлення

Перевірте, чи доступний Zephir з будь-якого каталогу за допомогою такої команди:

```bash
zephir help
```