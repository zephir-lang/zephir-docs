# Навчальний посібник

Zephir, і цей посібник, призначені для PHP-розробників, які хочуть створити C-розширення, але не знають C.

Ми припускаємо, що ви маєте досвід в одній або декількох інших мовах програмування. Ми проведемо паралелі з функціями PHP, C, JavaScript та іншими мовами. Ми акцентуватимемо увагу на функції Zephir, які є подібними до функцій в інших мовах, а також на функціях які є новими. Якщо ви знайомі з цими мовами вам буде простіше розібратися що до чого.

У цьому посібнику ми будемо використовувати стандартний термінал команд Linux. Якщо ви користувач Windows, потрібно замінити ці команди на аналогічні.

<a name='checking-the-installation'></a>

## Перевірка встановлення

Якщо ви успішно встановили Zephir, тоді виконайте таку команду в консолі:

```bash
zephir help
```

Якщо все гаразд, ви повинні побачити такий список команд та логотип (або щось схоже):

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
    

Якщо щось пішло не так, будь ласка, поверніться на сторінку [Встановлення](/[[language]]/[[version]]/installation).

<a name='extension-skeleton'></a>

## Каркас розширення

Перше, що нам потрібно зробити це згенерувати каркас розширення. Це створить базову структуру нашого розширення з якою ми працюватимемо далі. У нашому випадку ми створимо розширення під назвою `utils` (утиліти):

```bash
zephir init utils
```

Після цього в поточному робочому каталозі створиться каталог, що називається «utils»:

    utils/
       ext/
       utils/
    

Каталог `ext/` (всередині utils) містить код, який буде використовуватися компілятором для створення розширення. У каталозі є ще один каталог, який має таке ж ім'я, як і наше розширення, а саме `utils`. Ми розмістимо код Zephir в цьому каталозі.

Нам необхідно змінити робочий каталог на «utils», щоб почати компіляцію нашого коду:

```bash
cd utils
ls
ext/ utils/ config.json
```

Перегляд списку файлів покаже нам файл `config.json`. Цей файл містить параметри конфігурації, які ми можемо використовувати, щоб змінювати поведінку Zephir та/або розширення.

<a name='adding-our-first-class'></a>

## Додавання нашого першого класу

Zephir спроектований створювати об'єктно-орієнтовані розширення. Щоб почати розробку, нам потрібно додати наш перший клас до розширення.

Як і в багатьох інших мовах чи інструментах, перше, що ми хочемо побачити `Привіт світ` згенерований Zephir та перевірити чи все гаразд. Отже, наш перший клас називатиметься `Utils\Greeting` і міститиме метод, який друкуватиме `Привіт світ!`.

Код цього класу повинен бути поміщеним у `utils/utils/greeting.zep`:

```zep
namespace Utils;

class Greeting
{

    public static function say()
    {
        echo "Привіт світ!";
    }

}
```

Тепер ми маємо сказати Zephir, що наш проект треба зібрати та згенерувати розширення:

```bash
zephir build
```

Спочатку, і лише першого разу, виконається ряд внутрішніх команд, які створять необхідний код та конфігурації для експортування цього класу в PHP-розширення. Якщо все пройде без проблем в кінці ви побачите таке повідомлення:

    ...
    Extension installed!
    Add extension=utils.so to your php.ini
    Don't forget to restart your web server
    

На цьому етапі, швидше за все, для встановлення розширення вам потрібно буде ввести ваш root пароль.

Зрештою, вам залишиться лише під'єднати ваше розширення у `php.ini` та перезапустити PHP-сервер. Щоб під'єднати ваше розширення, потрібно додати директиву `extension=utils.so` до файлу php.ini.

Примітка: Ви також можете під'єднати розширення через консоль за допомогою команди `-d extension=utils.so`, але це під'єднає його лише для одного запиту і вам доведеться писати цю команду для кожного запиту. Додавання ж директиви до `php.ini` забезпечить підключення вашого розширення для кожного запиту.

<a name='initial-testing'></a>

## Первинне тестування

Тепер, коли розширення було додано до вашого `php.ini`, перевірте правильність завантаження розширення, виконавши наступну команду:

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

Розширення `utils` повинно бути частиною виходу, що означає, що розширення завантажено правильно. Now, let's see our `hello world` directly executed by PHP. To accomplish this, you can create a simple PHP file calling the static method we have just created:

```php
<?php

echo Utils\Greeting::say(), "\n";
```

Congratulations! Уou have your first extension running in PHP.

<a name='a-useful-class'></a>

## Корисний клас

The `Utils\Greeting::say` method was fine to check if our environment was right. Now, let's create some more useful classes.

The first useful class we are going to add to this extension will provide filtering facilities to users. This class is called `Utils\Filter` and its code must be placed in `utils/utils/filter.zep`:

A basic skeleton for this class is the following:

```zep
namespace Utils;

class Filter
{

}
```

The class contains filtering methods that help users to filter unwanted characters from strings. The first method is called `alpha`, and its purpose is to filter only those characters that are ASCII basic letters. To begin, we are just going to traverse the string, printing every byte to the standard output:

```zep
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

When invoking this method:

```php
<?php

$f = new Utils\Filter();
$f->alpha("привіт");
```

You will see:

    п
    р
    и
    в
    і
    т
    

Checking every character in the string is straightforward. Now we'll create another string with the right filtered characters:

```zep
class Filter
{

    public function alpha(string str) -> string
    {
        char ch; string filtered = "";

        for ch in str {
            if (ch >= 'а' && ch <= 'я') || (ch >= 'А' && ch <= 'Я') {
                let filtered .= ch;
            }
        }

        return filtered;
    }
}
```

The complete method can be tested as before:

```php
<?php

$f = new Utils\Filter();
echo $f->alpha("!пр#02и3'121віт."); // надрукує "привіт"
```

In the following screencast you can watch how to create the extension explained in this tutorial: <iframe src="//player.vimeo.com/video/84180223" width="500" height="313" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen mark="crwd-mark"></iframe> 

<a name='conclusion'></a>

## Висновок

This is a very simple tutorial, and as you can see, it's easy to start building extensions using Zephir. We invite you to continue reading the manual so that you can discover additional features offered by Zephir!