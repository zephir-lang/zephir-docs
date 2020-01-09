---
layout: default
language: 'ru-ru'
version: '0.12'
---

# Classes and Objects

Zephir promotes object-oriented programming. Вот почему вы можете экспортировать только методы и классы в расширениях. Также вы увидите, что в большинстве случаев, ошибки времени исполнения порождают исключения вместо фатальных ошибок или предупреждений.

<a name='classes'></a>

## Классы

Every Zephir file must implement a class or an interface (and just one). A class structure is very similar to a PHP class:

```zep
/**
 * Это простой класс
 */
class MyClass
{

}
```

<a name='classes-modifiers'></a>

### Модификаторы класса

Поддерживаются следующие модификаторы класса:

`final`: If a class has this modifier it cannot be extended:

```zep
namespace Test;

/**
 * This class cannot be extended by another class
 */
final class MyClass
{

}
```

`abstract`: если класс имеет этот модификатор, то его экземпляр не может быть создан:

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

### Реализация интерфейсов

Zephir classes can implement any number of interfaces, provided that these interfaces are `visible` for the class to use. Однако, есть случаи, когда класс Zephir (и впоследствии расширение) может потребоваться для реализации интерфейса, который создан в другом расширении.

If we want to implement the `MiddlewareInterface` from the `PSR` extension, we will need to create a `stub` interface:

```zep
// middlewareinterfaceex.zep
namespace Test\Oo\Extend;

use Psr\Http\Server\MiddlewareInterface;

interface MiddlewareInterfaceEx extends MiddlewareInterface
{

}
```

From here we can use the `stub` interface throughout our extension.

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

**NOTE** It is the developer's responsibility to ensure that all external references are present before the extension is loaded. So for the example above, one has to load the [PSR](https://pecl.php.net/package/psr) extension **first** before the Zephir built extension is loaded.

<a name='implementing-methods'></a>

## Реализация методов

The `function` keyword introduces a method. Methods implement the usual visibility modifiers available in PHP. Explicitly setting a visibility modifier is mandatory in Zephir:

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

Methods can receive required and optional parameters:

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

### Optional nullable parameters

Zephir гарантирует, что значение переменной останется такого же типа, с которым она была объявлена. This makes Zephir convert the `null` value to the closest approximate value:

```zep
public function foo(int a = null)
{
    echo a; // Если "a" не передано, выведет 0
}

public function foo(boolean a = null)
{
    echo a; //  Если "a" не передано, выведет false
}

public function foo(string a = null)
{
    echo a; //  Если "a" не передано, выведет пустую строку
}

public function foo(array a = null)
{
    var_dump(a); //  Если "a" не передано, выведет пустой массив
}
```

<a name='implementing-methods-supported-visibilities'></a>

### Supported Visibilities

* Public: Methods marked as `public` are exported to the PHP extension; this means that public methods are visible to the PHP code as well to the extension itself.

* Protected: Methods marked as `protected` are exported to the PHP extension; this means that protected methods are visible to the PHP code as well to the extension itself. However, protected methods can only be called in the scope of the class or in classes that inherit them.

* Private: Methods marked as `private` are not exported to the PHP extension; this means that private methods are only visible to the class where they're implemented.

<a name='implementing-methods-supported-modifiers'></a>

### Поддерживаемые модификаторы

* `static`: Methods with this modifier can only be called in a static context (from the class, not an object).

* `final`: If a method has this modifier it cannot be overriden.

* `deprecated`: Methods marked as `deprecated` throw an `E_DEPRECATED` error when they are called.

<a name='implementing-methods-getter-setter-shortcuts'></a>

### Getter/Setter shortcuts

Как и в C#, в Zephir вы можете использовать `get`/`set`/`toString` сокращения. Эта особенность позволяет легко писать геттеры и сеттеры для свойств, без явной реализации этих методов как таковых.

For example, without shortcuts we would need code like:

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

You can write the same code using shortcuts as follows:

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

Когда код скомпилирован, эти методы экспортируются в качестве реальных методов и их не нужно писать вручную.

<a name='implementing-methods-return-type-hints'></a>

### Return Type Hints

Methods in classes and interfaces can have "return type hints". Это позволит компилятору предоставить полезную дополнительную информацию об ошибках в вашем приложении. Рассмотрим следующий пример:

```zep
namespace App;

class MyClass
{
    public function getSomeData() -> string
    {
        // this will throw a compiler exception
        // since the returned value (boolean) does not match
        // the expected returned type string
        return false;
    }

    public function getSomeOther() -> <App\MyInterface>
    {
        // this will throw a compiler exception
        // if the returned object does not implement
        // the expected interface App\MyInterface
        return new App\MyObject;
    }

    public function process()
    {
        var myObject;

        // the type-hint will tell the compiler that
        // myObject is an instance of a class
        // that implement App\MyInterface
        let myObject = this->getSomeOther();

        // the compiler will check if App\MyInterface
        // implements a method called "someMethod"
        echo myObject->someMethod();
    }
}
```

Метод может иметь более одного возвращаемого типа. Когда определены несколько типов, для их разделения используется оператор `|`.

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

### Возвращаемый тип: Void

Методы могут быть также помечены как `void`. This means that a method is not allowed to return any data:

```zep
public function setConnection(connection) -> void
{
    let this->_connection = connection;
}
```

Why is this useful? Потому что компилятор может определить, ожидает ли программа возврата значения из этих методов, и вызовет исключение компилятора:

```zep
let myDb = db->setConnection(connection); // this will produce an exception
myDb->execute("SELECT * FROM robots");
```

<a name='implementing-methods-strict-flexible-parameter-data-types'></a>

### Strict/Flexible Parameter Data-Types

В Zephir вы можете указать тип данных каждого параметра метода. By default, these data-types are flexible; this means that if a value with a wrong (but compatible) data-type is passed, Zephir will try to transparently convert it to the expected one:

```zep
public function filterText(string text, boolean escape=false)
{
    //...
}
```

Above method will work with the following calls:

```zep
<?php

$o->filterText(1111, 1);              // OK
$o->filterText("some text", null);    // OK
$o->filterText(null, true);           // OK
$o->filterText("some text", true);    // OK
$o->filterText(array(1, 2, 3), true); // FAIL
```

Однако, передача неправильного типа может часто приводить к ошибкам. Неправильное использование конкретного API может привести к неожиданным результатам. Вы можете запретить автоматическое преобразование, установив параметр со строгим типом данных:

```zep
public function filterText(string! text, boolean escape=false)
{
    //...
}
```

Теперь, большинство вызовов с неправильным типом приведет к исключению из-за неправильных типов передаваемых данных:

```zep
<?php

$o->filterText(1111, 1);              // FAIL
$o->filterText("some text", null);    // OK
$o->filterText(null, true);           // FAIL
$o->filterText("some text", true);    // OK
$o->filterText(array(1, 2, 3), true); // FAIL
```

By specifying what parameters are strict and what can be flexible, a developer can set the specific behavior he/she really wants.

<a name='implementing-methods-read-only-parameters'></a>

### Read-Only Parameters

При помощи ключевого слова `const`, вы можете пометить параметры как «только для чтения», это помогает соблюдать «[const-корректность](https://en.wikipedia.org/wiki/Const_(computer_programming))». Parameters marked with this attribute cannot be modified inside the method:

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

Когда параметр объявлен только для чтения, компилятор может делать безопасные предположения и проводить дальнейшие оптимизации этих переменных.

<a name='implementing-properties'></a>

## Реализация свойств

Class member variables are called "properties". По умолчанию, они действуют так же, как и свойства PHP. Properties are exported to the PHP extension, and are visible from PHP code. Properties implement the usual visibility modifiers available in PHP, and explicitly setting a visibility modifier is mandatory in Zephir:

```zep
namespace Test;

class MyClass
{
    public myProperty1;

    protected myProperty2;

    private myProperty3;
}
```

Within class methods, non-static properties may be accessed by using `->` (Object Operator):

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

Properties can have literal compatible default values. These values must be able to be evaluated at compile time and must not depend on run-time information in order to be evaluated:

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

## Обновление свойств

Свойства могут быть обновлены, при помощи оператора `->`:

```zep
let this->myProperty = 100;
```

Zephir checks that properties exist when a program is accessing them. Если свойство не объявлено, вы получите исключение компилятора:

```bash
CompilerException: Property '_optionsx' is not defined on class 'App\MyClass' in /Users/scott/utils/app/myclass.zep on line 62

      let this->_optionsx = options;
      ------------^
```

Если вы хотите избежать проверки компилятором или просто создать свойство динамически, вы можете заключить имя свойства в фигурные скобки и кавычки:

```zep
let this->{"myProperty"} = 100;
```

Вы также можете использовать простую переменную для обновления свойств; имя свойства будет взято из переменной:

```zep
let someProperty = "myProperty";
let this->{someProperty} = 100;
```

<a name='implementing-properties-reading'></a>

## Чтение свойств

Свойства могут быть прочитаны при помощи оператора `->`:

```zep
echo this->myProperty;
```

Как и при обновлении, свойства могут быть динамически прочитаны следующим образом:

```zep
// Avoid compiler check or read a dynamic user defined property
echo this->{"myProperty"};

// Read using a variable name
let someProperty = "myProperty";
echo this->{someProperty}
```

<a name='class-constants'></a>

## Class Constants

Классы могут содержать константы классов, которые остаются неизменными после компиляции расширения. Константы класса экспортируются в PHP-расширение, позволяя использовать их из PHP.

```zep
namespace Test;

class MyClass
{
    const MYCONSTANT1 = false;
    const MYCONSTANT2 = 1.0;
}
```

Class constants can be accessed using the class name and the static operator `::`:

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

## Вызов методов

Методы могут выываться с помощью объектного оператора `->` как в PHP:

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

Статические методы должны вызываться с помощью статического оператора `::`:

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

Вы можете вызывать методы динамически следующим образом:

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

### Доступ к параметрам по имени

Zephir поддерживает вызов параметров метода по имени или аргументам ключевого слова. Named parameters can be useful if you want to pass parameters in an arbitrary order, document the meaning of parameters, or specify parameters in a more elegant way.

Рассмотрим следующий пример. Класс с именем `Image` имеет метод, который получает четыре параметра:

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

Использование стандартного метода вызова:

```zep
i->chop(100);             // width=100, height=400, x=0, y=0
i->chop(100, 50, 10, 20); // width=100, height=50, x=10, y=20
```

Использование именованных параметров:

```zep
i->chop(width: 100);              // width=100, height=400, x=0, y=0
i->chop(height: 200);             // width=600, height=200, x=0, y=0
i->chop(height: 200, width: 100); // width=100, height=200, x=0, y=0
i->chop(x: 20, y: 30);            // width=600, height=400, x=20, y=30
```

Когда компилятор (во время компиляции) не знает правильного порядка этих параметров, они должны быть определены во время выполнения. В этом случае могут возникнуть минимальные накладные расходы:

```zep
let i = new {someClass}();
i->chop(y: 30, x: 20);
```