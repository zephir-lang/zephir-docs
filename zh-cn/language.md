* * *

layout: default language: 'en' version: '0.10' menu:

- text: 'Organizing code in files and namespaces' url: '#organizing-code-in-files-and-namespaces'
- text: 'Instruction separation' url: '#instruction-separation'
- text: 'Comments' url: '#comments'
- text: 'Variable declarations' url: '#variable-declarations'
- text: 'Variable scope' url: '#variable-scope'
- text: 'Super globals' url: '#super-globals'
- text: 'Local symbol table' url: '#local-symbol-table'

* * *

# 基本语法

在本章中, 我们将讨论文件和命名空间、变量声明、杂项语法约定以及其他几个一般概念的组织。

<a name='organizing-code-in-files-and-namespaces'></a>

## 在文件和命名空间中组织代码

在 php 中, 您可以将代码放置在任何文件中, 而不需要特定的结构。 在 Zephir中, 每个文件都必须包含一个类 (并且只有一个类)。 每个类都必须有一个命名空间, 并且目录结构必须与所使用的类和命名空间的名称相匹配。 (这类似于 psr-4 自动加载约定, 只是它是由语言本身强制执行的。

例如, 给定以下结构, 每个文件中的类必须是:

```bash
mylibrary/
    router/
        exception.zep # 
    router.zep # MyLibrary\Router
```

Class in `mylibrary/router.zep`:

```zephir
namespace MyLibrary;

class Router
{

}
```

Class in `mylibrary/router/exception.zep`:

```zephir
namespace MyLibrary\Router;

class Exception extends \Exception
{

}
```

如果文件或类不在预期文件中, 则 Zephir 将引发编译器异常, 反之亦然。

<a name='instruction-separation'></a>

## 指令分离

您可能已经注意到, 前一章中的代码示例中很少有分号。 您可以使用分号分隔语句和表达式, 如 java、c/c ++、php 和类似语言:

```zephir
myObject->myMethod(1, 2, 3); echo "world";
```

<a name='comments'></a>

## 注释

Zephir 支持 "c"/"c++" 注释。 这是行注释 `// ...`, 这是多行注释 `/* ... */`:

```zephir
// this is a one line comment

/**
 * multi-line comment
 */
```

在大多数语言中，注释只是编译器/解释器忽略的文本。 在Zephir中，多行注释也用作docblock，它们被导出到生成的代码中，因此它们是语言的一部分!

如果docblock不在预期的位置，编译器将抛出异常。

<a name='variable-declarations'></a>

## 变量声明

在Zephir中，必须声明给定范围中使用的所有变量。 这为编译器执行优化和验证提供了重要信息。 变量必须是唯一的标识符，它们不能是保留字。

```zephir
// 在同一指令中声明相同类型的变量
var a, b, c;

// 在单独的行中声明每个变量
var a;
var b;
var c;
```

变量可以选择有一个初始兼容的默认值:

```zephir
// 使用默认值声明变量
var a = "hello", b = 0, c = 1.0;
int d = 50; bool some = true;
```

变量名区分大小写，以下变量不同:

```zephir
// 不同的变量
var somevalue, someValue, SomeValue;
```

<a name='variable-scope'></a>

## 变量作用域

所有声明的变量都局部作用于声明它们的方法:

```zephir
namespace Test;

class MyClass
{
    public function someMethod1()
    {
        int a = 1, b = 2;
        return a + b;
    }

    public function someMethod2()
    {
        int a = 3, b = 4;
        return a + b;
    }
}
```

<a name='super-global'></a>

## 超全局

Zephir不支持全局变量——不允许从PHP代码块访问全局变量。 然而，您可以访问PHP的超全局变量，如下所示:

```zephir
// 从_POST获取值
let price = _POST["price"];

// 从_SERVER读取值
let requestMethod = _SERVER["REQUEST_METHOD"];
```

<a name='local-symbol-table'></a>

## 本地符号表

PHP中的每个方法或上下文都有一个符号表，允许您以非常动态的方式编写变量:

```php
<?php

$b = 100;
$a = "b";
echo $$a; // prints 100
```

Zephir没有实现这个特性，因为所有变量都被编译为低级变量，而且无法知道在特定上下文中存在哪些变量。 如果您想在当前PHP符号表中创建一个变量，您可以使用以下语法:

```zephir
// Set variable $name in PHP
let {"name"} = "hello";

// Set variable $price in PHP
let name = "price";
let {name} = 10.2;
```