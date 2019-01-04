---
layout: default
language: 'zh-cn'
version: '0.11'
menu:
  - text:
      '类'
    url: '#classes'
    sub:
      - text:
            '类修饰符'
        url: '#classes-modifiers'
      - text:
            '实现接口'
        url: '#classes-interfaces'
  - text:
      '实现方法'
    url: '#implementing-methods'
    sub:
      - text:
            '可选参数可以为空'
        url: '#implementing-methods-optional-nullable-parameters'
      - text:
            '支持可见性'
        url: '#implementing-methods-supported-visibilities'
      - text:
            '支持修改器'
        url: '#implementing-methods-supported-modifiers'
      - text:
            'Getter / Setter 快捷方式'
        url: '#implementing-methods-getter-setter-shortcuts'
      - text:
            '返回类型提示'
        url: '#implementing-methods-return-type-hints'
      - text:
            '返回类型: Void'
        url: '#implementing-methods-return-type-void'
      - text:
            '严格/灵活的参数的数据类型'
        url: '#implementing-methods-strict-flexible-parameter-data-types'
      - text:
            '只读参数'
        url: '#implementing-methods-read-only-parameters'
  - text:
      '实现属性'
    url: '#implementing-properties'
    sub:
      - text:
            '更新属性'
        url: '#implementing-properties-updating'
      - text:
            '查看属性'
        url: '#implementing-properties-reading'
  - text:
      '类常量'
    url: '#class-constants'
  - text:
      '调用方法'
    url: '#calling-methods'
    sub:
      - text:
            '参数名'
        url: '#calling-methods-parameters-by-name'
---
# 类与对象

Zephir 提倡面向对象编程。 这就是为什么您只能在扩展中导出方法和类的原因。 您还将看到, 在大多数情况下, 运行时错误会引发异常, 而不是致命错误或警告。

<a name='classes'></a>

## 类

每个 Zephir 文件都必须实现一个类或接口 (并且只有一个)。 类结构与 php 类非常相似:

```zep
namespace Test;

/**
 * This is a sample class
 */
class MyClass
{

}
```

<a name='classes-modifiers'></a>

### 类修饰符

支持以下类修饰符:

`final`: 如果一个类有这个修饰符，它就不能被扩展:

```zep
namespace Test;

/**
 * This class cannot be extended by another class
 */
final class MyClass
{

}
```

`abstract`: 如果一个类有这个修饰符，它不能被实例化:

```zep
namespace Test;

/**
 * This class cannot be instantiated
 */
abstract class MyClass
{

}
```

<a name='classes-interfaces'></a>

### 实现接口

Zephir 类可以实现任意数量的接口, 前提是这些接口 `显性` 的引用(使用use)。 但是, 有时 Zephir 类 (以及随后的扩展) 可能需要实现在不同扩展中构建的接口。

如果我们想要实现`MiddlewareInterface`从`PSR`扩展，我们需要创建一个`stub`接口:

```zep
// middlewareinterfaceex.zep
namespace Test\Oo\Extend;

use Psr\Http\Server\MiddlewareInterface;

interface MiddlewareInterfaceEx extends MiddlewareInterface
{

}
```

从这里开始，我们可以在整个扩展过程中使用`stub`接口。

```php
/**
 * @test
 */
public function shouldExtendMiddlewareInterface()
{
    if (!extension_loaded('psr')) {
        $this->markTestSkipped(
            "The psr extension is not loaded"
        );
    }

    $this->assertTrue(
        is_subclass_of(MiddlewareInterfaceEx::class, 'Psr\Http\Server\MiddlewareInterface')
    );
}
```

**NOTE**开发人员有责任确保在加载扩展之前存在所有外部引用。 因此，对于上面的示例，在加载Zephir构建的扩展之前，必须先加载[PSR](https://pecl.php.net/package/psr)扩展。

<a name='implementing-methods'></a>

## 实现方法

`function`关键字引入了一个方法。 方法实现PHP中常用的可见性修饰符。 在 Zephir中, 必须明确设置可见性修改器:

```zep
namespace Test;

class MyClass
{

    public function myPublicMethod()
    {
        // ...
    }

    protected function myProtectedMethod()
    {
        // ...
    }

    private function myPrivateMethod()
    {
        // ...
    }
}
```

方法可以接收所需的和可选的参数:

```zep
namespace Test;

class MyClass
{

    /**
     * All parameters are required
     */
    public function doSum1(a, b)
    {
        return a + b;
    }

    /**
     * Only 'a' is required, 'b' is optional and it has a default value
     */
    public function doSum2(a, b = 3)
    {
        return a + b;
    }

    /**
     * Both parameters are optional
     */
    public function doSum3(a = 1, b = 2)
    {
        return a + b;
    }

    /**
     * Parameters are required and their values must be integer
     */
    public function doSum4(int a, int b)
    {
        return a + b;
    }

    /**
     * Static typed with default values
     */
    public function doSum4(int a = 4, int b = 2)
    {
        return a + b;
    }
}
```

<a name='implementing-methods-optional-nullable-parameters'></a>

### 可选参数可以为空

Zephir确保变量的值保持声明的类型不变。 这使得Zephir将 `null`值转换为最近的近似值:

```zep
public function foo(int a = null)
{
    echo a; // if "a" is not passed it prints 0
}

public function foo(boolean a = null)
{
    echo a; // if "a" is not passed it prints false
}

public function foo(string a = null)
{
    echo a; // if "a" is not passed it prints an empty string
}

public function foo(array a = null)
{
    var_dump(a); // if "a" is not passed it prints an empty array
}
```

<a name='implementing-methods-supported-visibilities'></a>

### 支持可见性

* Public: 将标记为` public`的方法导出到PHP扩展; 这意味着公共方法对PHP代码和扩展本身都是可见的。

* Protected: 将标记为`protected`的方法导出到PHP扩展; 这意味着受保护的方法对PHP代码和扩展本身都是可见的。 但是，受保护的方法只能在类的作用域中调用，或者在继承它们的类中调用。

* Private: 标记为 `private` 的方法不会导出到PHP扩展; 这意味着私有方法只对实现它们的类可见。

<a name='implementing-methods-supported-modifiers'></a>

### 支持修改器

* `static`: 使用此修饰符的方法只能在静态上下文(来自类，而不是对象) 中调用。

* `final`: 如果方法具有此修饰符, 则无法对其进行覆盖.

* `deprecated`: 被标记为`deprecated`的方法在被调用时抛出一个`E_DEPRECATED`错误。

<a name='implementing-methods-getter-setter-shortcuts'></a>

### Getter / Setter 快捷方式

和C#一样，在Zephir中可以使用`get`/`set`/`toString`快捷方式。 该特性允许您轻松编写属性的setter和getter，而无需显式地实现这些方法。

例如，如果没有快捷方式，我们需要如下代码:

```zep
namespace Test;

class MyClass
{
    protected myProperty;

    protected someProperty = 10;

    public function setMyProperty(myProperty)
    {
        let this->myProperty = myProperty;
    }

    public function getMyProperty()
    {
        return this->myProperty;
    }

    public function setSomeProperty(someProperty)
    {
        let this->someProperty = someProperty;
    }

    public function getSomeProperty()
    {
        return this->someProperty;
    }

    public function __toString()
    {
        return this->myProperty;
    }
}
```

您可以使用以下快捷方式编写相同的代码:

```zep
namespace App;

class MyClass
{
    protected myProperty {
        set, get, toString
    };

    protected someProperty = 10 {
        set, get
    };
}
```

在编译代码时，这些方法作为实际方法导出，但您不必手动编写它们。

<a name='implementing-methods-return-type-hints'></a>

### 返回类型提示

类和接口中的方法可以有“返回类型提示”。 这些将为编译器提供有用的额外信息，以告知您应用程序中的错误。 请考虑下面的示例:

```zep
namespace App;

class MyClass
{
    public function getSomeData() -> string
    {
        // 这会引发编译器异常
        // 因为返回的值(布尔值)不匹配
        // 预期返回的类型字符串
        return false;
    }

    public function getSomeOther() -> <App\MyInterface>
    {
        //  这会引发编译器异常
        // 如果返回的对象没有实现
        // 期望的接口App\MyInterface
        return new App\MyObject;
    }

    public function process()
    {
        var myObject;

        // 类型提示会告诉编译器这一点
        // myObject是一个类的实例
        // 实现应用程序\MyInterface
        let myObject = this->getSomeOther();

        // 编译器会检查App\MyInterface是否正确
        // 实现一个名为“someMethod”的方法
        echo myObject->someMethod();
    }
}
```

一个方法可以有多个返回类型。 在定义多个类型时，必须使用操作符 `|`来分隔这些类型。

```zep
namespace App;

class MyClass
{
    public function getSomeData(a) -> string | bool
    {
        if a == false {
            return false;
        }
        return "error";
    }
}
```

<a name='implementing-methods-return-type-void'></a>

### 返回类型: Void

方法也可以标记为 `void`。 这意味着不允许方法返回任何数据:

```zep
public function setConnection(connection) -> void
{
    let this->_connection = connection;
}
```

为什么这是有用的？ 因为编译器可以检测程序是否期望从这些方法返回值，并产生一个编译器异常:

```zep
let myDb = db->setConnection(connection); // 这将产生一个异常
myDb->execute("SELECT * FROM robots");
```

<a name='implementing-methods-strict-flexible-parameter-data-types'></a>

### 严格/灵活的参数的数据类型

在 Zephir中, 可以指定方法的每个参数的数据类型。 默认情况下, 这些数据类型是灵活的。这意味着, 如果传递了具有错误 (但兼容) 数据类型的值, 则 Zephir 将尝试以透明方式将其转换为预期的数据类型:

```zep
public function filterText(string text, boolean escape=false)
{
    //...
}
```

上述方法将适用于以下调用:

```zep
<?php

$o->filterText(1111, 1);              // OK
$o->filterText("some text", null);    // OK
$o->filterText(null, true);           // OK
$o->filterText("some text", true);    // OK
$o->filterText(array(1, 2, 3), true); // FAIL
```

但是, 传递错误的类型通常会导致错误。 不正确地使用特定的 api 会产生意外的结果。 通过使用严格的数据类型设置参数, 可以禁止自动转换:

```zep
public function filterText(string! text, boolean escape=false)
{
    //...
}
```

现在，由于传递的数据类型无效，大多数类型错误的调用都会导致异常:

```zep
<?php

$o->filterText(1111, 1);              // FAIL
$o->filterText("some text", null);    // OK
$o->filterText(null, true);           // FAIL
$o->filterText("some text", true);    // OK
$o->filterText(array(1, 2, 3), true); // FAIL
```

通过指定严格的参数和灵活的参数，开发人员可以设置他/她真正想要的特定行为。

<a name='implementing-methods-read-only-parameters'></a>

### 只读参数

使用关键字`const`，您可以将参数标记为只读，这有助于尊重[const-correctness](http://en.wikipedia.org/wiki/Const-correctness)。 属性中不能修改标有此属性的参数 方法:

```zep
namespace App;

class MyClass
{
    // "a" is read-only
    public function getSomeData(const string a)
    {
        // this will throw a compiler exception
        let a = "hello";
    }
}
```

当一个参数被声明为只读时，编译器可以做出安全的假设，并对这些变量进行进一步的优化。

<a name='implementing-properties'></a>

## 实现属性

类成员变量称为 "属性"。 默认情况下, 它们的作用与 php 属性相同。 属性被导出到PHP扩展中，并从PHP代码中可见。 属性实现 php 中可用的常规可见性修饰符, 并且在 Zephir中必须显式设置可见性修饰符:

```zep
namespace Test;

class MyClass
{
    public myProperty1;

    protected myProperty2;

    private myProperty3;
}
```

在类方法中, 可以使用`->` (对象运算符) 访问非静态属性:

```zep
namespace Test;

class MyClass
{

    protected myProperty;

    public function setMyProperty(var myProperty)
    {
        let this->myProperty = myProperty;
    }

    public function getMyProperty()
    {
        return this->myProperty;
    }
}
```

属性可以具有文本兼容的默认值。 这些值必须能够在编译时进行计算, 并且不能依赖于运行时信息才能进行计算:

```zep
namespace Test;

class MyClass
{

    protected myProperty1 = null;
    protected myProperty2 = false;
    protected myProperty3 = 2.0;
    protected myProperty4 = 5;
    protected myProperty5 = "my value";
}
```

<a name='implementing-properties-updating'></a>

## 更新属性

属性可以通过使用 "->" 运算符访问来更新:

```zep
let this->myProperty = 100;
```

Zephir 检查程序在访问属性时会检查是否存在属性。 如果未声明属性, 您将获得编译器异常:

```bash
CompilerException: Property '_optionsx' is not defined on class 'App\MyClass' in /Users/scott/utils/app/myclass.zep on line 62

      let this->_optionsx = options;
      ------------^
```

如果要避免此编译器验证, 或者只是动态创建属性, 则可以使用括号和字符串引号将属性名称括起来:

```zep
let this->{"myProperty"} = 100;
```

您还可以使用一个简单的变量来更新属性; 属性名将取自变量:

```zep
let someProperty = "myProperty";
let this->{someProperty} = 100;
```

<a name='implementing-properties-reading'></a>

## 查看属性

属性可以通过使用 "->" 运算符访问来查看:

```zep
echo this->myProperty;
```

与更新一样, 可以通过以下方式动态读取属性:

```zep
// 避免编译器检查或读取动态用户定义属性
echo this->{"myProperty"};

// 使用变量名读取
let someProperty = "myProperty";
echo this->{someProperty}
```

<a name='class-constants'></a>

## 类常量

类可能包含类常量, 这些常量在编译扩展后保持不变和不可更改。 类常量导出到 php 扩展, 允许从 php 中使用它们。

```zep
namespace Test;

class MyClass
{
    const MYCONSTANT1 = false;
    const MYCONSTANT2 = 1.0;
}
```

类常量可以使用类名和静态运算符 `::`访问:

```zep
namespace Test;

class MyClass
{

    const MYCONSTANT1 = false;
    const MYCONSTANT2 = 1.0;

    public function someMethod()
    {
        return MyClass::MYCONSTANT1;
    }
}
```

<a name='calling-methods'></a>

## 调用方法

方法可以使用对象操作符 `->` 调用，就像在PHP中:

```zep
namespace Test;

class MyClass
{
    protected function _someHiddenMethod(a, b)
    {
        return a - b;
    }

    public function someMethod(c, d)
    {
        return this->_someHiddenMethod(c, d);
    }
}
```

必须使用静态操作符 `::` 调用静态方法:

```zep
namespace Test;

class MyClass
{
    protected static function _someHiddenMethod(a, b)
    {
        return a - b;
    }

    public static function someMethod(c, d)
    {
        return self::_someHiddenMethod(c, d);
    }
}
```

您可以按照以下动态方式调用方法:

```zep
namespace Test;

class MyClass
{
    protected adapter;

    public function setAdapter(var adapter)
    {
        let this->adapter = adapter;
    }

    public function someMethod(var methodName)
    {
        return this->adapter->{methodName}();
    }
}
```

<a name='calling-methods-parameters-by-name'></a>

### 参数名

Zephir 支持按名称或关键字参数调用方法参数。 如果您希望以任意顺序传递参数、记录参数的含义或以更优雅的方式指定参数，那么命名参数可能非常有用。

请考虑下面的示例. 一个名为 `Image` 的类具有接收四个参数的方法:

```zep
namespace Test;

class Image
{
    public function chop(width = 600, height = 400, x = 0, y = 0)
    {
        //...
    }
}
```

使用标准方法调用方法:

```zep
i->chop(100);             // width=100, height=400, x=0, y=0
i->chop(100, 50, 10, 20); // width=100, height=50, x=10, y=20
```

使用命名参数, 您可以:

```zep
i->chop(width: 100);              // width=100, height=400, x=0, y=0
i->chop(height: 200);             // width=600, height=200, x=0, y=0
i->chop(height: 200, width: 100); // width=100, height=200, x=0, y=0
i->chop(x: 20, y: 30);            // width=600, height=400, x=20, y=30
```

当编译器(在编译时) 不知道这些参数的正确顺序时，必须在运行时解析它们。 在这种情况下，可能会有一个最小的额外额外开销:

```zep
let i = new {someClass}();
i->chop(y: 30, x: 20);
```
