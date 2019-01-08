---
layout: default
language: 'ru-ru'
version: '0.11'
---
# Урок

Zephir и это руководство предназначены для разработчиков PHP, которые хотят создавать C-расширения с меньшей сложностью.

Мы предполагаем, что вы знакомы с одним или несколькими языками программирования. Мы проводим параллели с функциями в PHP, C, Javascript и других языках. Мы будем указывать на функции в Zephir, которые схожи с их аналогами в других языках, а также на функции, поведение которых не является похожим или даже является новым. Если вы знаете какой-либо из этих языков, вы будете отмечать эти сходства и различия более быстро.

В этом руководстве мы будем использовать стандартные команды терминала Linux. Если вы являетесь пользователем Windows, вам нужно заменить эти команды их аналогами.

<a name='checking-the-installation'></a>

## Проверка установки

Если вы успешно установили Zephir, вы должны быть в состоянии выполнить следующую команду в своей консоли:

```bash
zephir help
```

Если все в порядке, на вашем экране должна появиться следующая справка (или очень похожая):

     _____              __    _
    /__  /  ___  ____  / /_  (_)____
      / /  / _ \/ __ \/ __ \/ / ___/
     / /__/  __/ /_/ / / / / / /
    /____/\___/ .___/_/ /_/_/_/
             /_/
    
    Zephir version 0.10.9a-dev
    
    Usage:
        command [options]
    
    Available commands:
        stubs               Generates extension PHP stubs
        install             Installs the extension (requires root password)
        version             Shows the Zephir version
        compile             Compile a Zephir extension
        api [--theme-path=/path][--output-directory=/path][--theme-options={json}|/path]Generates a HTML API
        init [namespace]    Initializes a Zephir extension
        fullclean           Cleans the generated object files in compilation
        builddev            Generate/Compile/Install a Zephir extension in development mode
        clean               Cleans the generated object files in compilation
        generate            Generates C code from the Zephir code
        help                Displays this help
        build               Generate/Compile/Install a Zephir extension
    
    Options:
        -f([a-z0-9\-]+)     Enables compiler optimizations
        -fno-([a-z0-9\-]+)  Disables compiler optimizations
        -w([a-z0-9\-]+)     Turns a warning on
        -W([a-z0-9\-]+)     Turns a warning off
    

Если что-то пошло не так, вернитесь к разделу [установка](/0.11/ru-ru/installation).

<a name='extension-skeleton'></a>

## Каркас расширения

Первое, что нам нужно сделать, это сгенерировать скелет расширения. Это предоставит нашему расширению базовую структуру, которую мы должны начать работать. В нашем случае мы создадим расширение под названием `utils`:

```bash
zephir init utils
```

После этого в текущем рабочем каталоге создается каталог с именем "utils":

```bash
utils/
   ext/
   utils/
```

Каталог `ext/` (внутри utils) содержит код, который будет использоваться компилятором для создания расширения. Другой созданный каталог — `utils`, этот каталог имеет то же самое, что и наше расширение. Мы разместим код Zephir в этом каталоге.

Нам нужно изменить рабочий каталог на "utils", чтобы начать компилировать наш код:

```bash
cd utils
ls
ext/ utils/ config.json
```

В листинге каталога также будет отображаться файл с именем `config.json`. Этот файл содержит параметры конфигурации, которые мы можем использовать для изменения поведения Zephir и/или этого расширения.

<a name='adding-our-first-class'></a>

## Добавление нашего первого класса

Zephir предназначен для создания объектно-ориентированных расширений. Чтобы начать разработку, нам нужно добавить наш первый класс к расширению.

Как и во многих языках/инструментах, первое, что мы хотим сделать, это увидеть `hello world`, сгенерированный Zephir, и проверить, что все в порядке. Итак, наш первый класс будет называться `Utils\Greeting` и он содержит метод печати `hello world!`.

Код для этого класса должен быть помещен в `utils/utils/greeting.zep`:

```zephir
namespace Utils;

class Greeting
{

    public static function say()
    {
        echo "hello world!";
    }

}
```

Теперь нам нужно сообщить Zephir, что наш проект должен быть скомпилирован и сгенерировано расширение:

```bash
zephir build
```

Изначально и только в первый раз выполняется ряд внутренних команд, создающих необходимый код и конфигурации, чтобы экспортировать этот класс в расширение PHP. Если все пойдет хорошо, вы увидите следующее сообщение в конце вывода:

```bash
    ...
Extension installed!
Add extension=utils.so to your php.ini
Don't forget to restart your web server
```

На этом этапе вполне вероятно, что вам потребуется указать пароль root, чтобы установить расширение.

Наконец, расширение должно быть добавлено в `php.ini` для загрузки PHP. Это достигается добавлением директивы инициализации: `extension=utils.so` к нему.

Замечание: Вы так же можете запускать PHP передавая ему в качестве опции `-d extension=utils.so`. Однако это одноразовый «трюк», вам необходимо будет поступать так каждый раз, тестируя ваше расширение в терминале. Добавление директивы в `php.ini` будет гарантировать, что расширение загружается для каждого запроса.

<a name='initial-testing'></a>

## Первоначальное тестирование

Теперь, когда расширение было добавлено в ваш `php.ini`, проверьте, правильно ли загружается расширение, выполнив следующее:

```bash
php -m
[PHP Modules]
Core
date
libxml
pcre
Reflection
session
SPL
standard
tokenizer
utils
xdebug
xml
```

Расширения `utils` должны быть частью вывода, указывающего, что расширение было загружено правильно. Теперь давайте посмотрим на наш `hello world`, непосредственно выполняемый PHP. Для этого вы можете создать простой PHP-файл, вызывающий статический метод, который мы только что создали:

```php
<?php

echo Utils\Greeting::say(), "\n";
```

Поздравляем! У вас есть первое расширение, работающее с PHP.

<a name='a-useful-class'></a>

## Удобные класс

Метод `Utils\Greeting::say` был хорош, чтобы проверить, правильно ли сконфигурирована наша среда. А теперь давайте создадим еще несколько полезных классов.

Первый полезный класс, который мы добавим к этому расширению, предоставит пользователям средства фильтрации. Этот класс называется `Utils\Filter`, и его код должен быть помещен в `utils/utils/filter.zep`:

Основным скелетом этого класса является следующее:

```zephir
namespace Utils;

class Filter
{

}
```

Класс содержит методы фильтрации, которые помогают пользователям фильтровать нежелательные символы из строк. Первый метод называется `alpha`, и его целью является отфильтровать только те символы, которые являются основными буквами ASCII. Для начала, мы просто пройдем через строковую печать каждого байта в стандартный вывод:

```zephir
namespace Utils;

class Filter
{

    public function alpha(string str)
    {
        char ch;

        for ch in str {
            echo ch, "\n";
        }
    }
}
```

При вызове этого метода:

```php
<?php

$f = new Utils\Filter();
$f->alpha("hello");
```

Вы увидите:

    h
    e
    l
    l
    o
    

Проверка каждого символа в строке проста. Теперь мы можем просто создать другую строку с правильными отфильтрованными символами:

```zephir
class Filter
{

    public function alpha(string str) -> string
    {
        char ch; string filtered = "";

        for ch in str {
            if (ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z') {
                let filtered .= ch;
            }
        }

        return filtered;
    }
}
```

Полный метод может быть проверен, как и прежде:

```php
<?php

$f = new Utils\Filter();
echo $f->alpha("!he#02l3'121lo."); // выведет "hello"
```

В следующем скринкасте вы можете посмотреть, как создать расширение, объяснённое в этом уроке: <iframe src="//player.vimeo.com/video/84180223" width="500" height="313" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen mark="crwd-mark"></iframe> 

<a name='conclusion'></a>

## Заключение

Это очень простой учебник, и, как вы можете видеть, легко начать создание расширений с помощью Zephir. Мы приглашаем вас продолжить чтение руководства, чтобы вы могли ознакомиться с дополнительными функциями, которые предлагает Zephir!
