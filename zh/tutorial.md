# 教程

Zephir和本手册是为希望创建 c 扩展的 php 开发人员准备的, 其复杂性较低。

我们假设您在一种或多种其他编程语言方面有丰富的经验。 我们与 php、c、javascript 和其他语言中的功能进行了比较。 我们将指出Zephir中与这些其他语言相似的特性，以及许多新的或不同的特性。 如果您熟悉这些特定的语言，您将更快地了解这些比较。

在本指南中, 我们将使用标准的 linux 终端命令。 如果您是 windows 用户, 则需要将这些命令替换为对应命令。

<a name='checking-the-installation'></a>

## 检查安装

如果您已成功安装 Zephir, 则可以在控制台中执行以下命令:

```bash
zephir 帮助
```

如果一切正常, 您应该看到以下帮助 (或非常相似的内容):

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
    

如果出现问题, 请返回到 [installation](/[[language]]/[[version]]/installation) 页面。

<a name='extension-skeleton'></a>

## 扩展骨架

我们要做的第一件事就是生成扩展骨架。 这将为我们的扩展提供我们开始工作所需的基本结构。 在我们的示例中, 我们将创建一个名为 `utils` 的扩展:

```bash
zephir初始化工具
```

在此之后, 将在当前工作目录上创建一个名为 "utils" 的目录:

    utils/
       ext/
       utils/
    

目录 `ext/` (内部实用程序) 包含编译器将用于生成扩展的代码。 创建的另一个目录是 `utils`-此目录与我们的扩展具有相同的名称。 我们将把 Zephir 代码放在那里。

我们需要将工作目录更改为 "utils", 以开始编译我们的代码:

```bash
cd utils
ls
ext/ utils/ config.json
```

目录列表还将向我们显示一个名为 `config.json` 的文件。 此文件包含配置设置, 我们可以使用这些设置来更改 Zephir 和/或扩展本身的行为。

<a name='adding-our-first-class'></a>

## 添加我们的第一个类

Zephir 旨在生成面向对象的扩展。 要开始开发功能, 我们需要将我们的第一个类添加到扩展中。

与许多语言工具一样, 我们首先要做的是看到 Zephir 生成的 "0>hello world< a0 > 0", 并检查一切是否正常。 因此, 我们的第一个类将被称为 `Utils\Greeting`, 并包含一个方法打印 < 0>hello world!</0 >。

此类的代码必须放在 `utils/utils/greeting.zep`:

```zep
namespace Utils;

class Greeting
{

    public static function say()
    {
        echo "hello world!";
    }

}
```

现在, 我们需要告诉 Zephir, 我们的项目必须编译并生成扩展:

```bash
zephir build
```

Initially, and only for the first time, a number of internal commands are executed producing the necessary code and configurations to export this class to the PHP extension. 如果一切顺利, 您将在输出的末尾看到以下消息:

    ...
    Extension installed!
    添加 extension=utils.so 到你的 php.ini
    不要忘记重启你的服务器
    

在上述步骤中, 您很可能需要提供根密码才能安装扩展。

最后, 必须将扩展添加到 `php.ini` 才能由 php 加载。 这是通过添加初始化指令来实现的:` extension=utils.so `。

注意:您还可以在命令行上使用`-d extension=utils.so `来加载它。 因此，但它只会为那个请求加载，所以每次在CLI中测试扩展时都需要包含它。 将指令添加到 `php.ini` 将确保从此以后为每个请求加载它。

<a name='initial-testing'></a>

## 初步测试

现在扩展已经添加到`php.ini`，执行以下操作检查扩展是否正确加载:

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

扩展`utils`应该是输出的一部分，表明扩展被正确加载。 现在，让我们看看由PHP直接执行的`hello world`。 为此，您可以创建一个简单的PHP文件，调用我们刚刚创建的静态方法:

```php
<?php

echo Utils\Greeting::say(), "\n";
```

祝贺您！ 在PHP运行你的第一个扩展。

<a name='a-useful-class'></a>

## 一个有用的类

`Utils Greeting::say`方法可以检查我们的环境是否正确。 Now, let's create some more useful classes.

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
$f->alpha("hello");
```

You will see:

    h
    e
    l
    l
    o
    

Checking every character in the string is straightforward. Now we'll create another string with the right filtered characters:

```zep
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

The complete method can be tested as before:

```php
<?php

$f = new Utils\Filter();
echo $f->alpha("!he#02l3'121lo."); // prints "hello"
```

In the following screencast you can watch how to create the extension explained in this tutorial: <iframe src="//player.vimeo.com/video/84180223" width="500" height="313" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen mark="crwd-mark"></iframe> 

<a name='conclusion'></a>

## 结语

This is a very simple tutorial, and as you can see, it's easy to start building extensions using Zephir. We invite you to continue reading the manual so that you can discover additional features offered by Zephir!