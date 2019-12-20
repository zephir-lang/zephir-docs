---
layout: default
language: 'ru-ru'
version: '0.10'
---

# Классы и объекты
Zephir продвигает объектно-ориентированное программирование. Вот почему вы можете экспортировать только методы и классы в расширениях. Также вы увидите, что в большинстве случаев, ошибки времени исполнения порождают исключения вместо фатальных ошибок или предупреждений.

<a name='classes'></a>

## Классы
Каждый Zephir-файл должен реализовывать класс или интерфейс (причём только один). Структура класса крайне схожа с структурой обьявления его в PHP:

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

`final`: если класс имеет этот модификатор, он не сможет быть расширен (унаследован):

```zep
namespace Test;

/**
 * Этот класс не может быть расширен другим классом
 */
final class MyClass
{

}
```

`abstract`: если класс имеет этот модификатор, то его экземпляр не может быть создан:

```zep
namespace Test;

/**
 * Экземпляр этого класса не может быть создан
 */
abstract class MyClass
{

}
```

<a name='classes-interfaces'></a>

### Реализация интерфейсов
Классы в Zephir могут реализовать любое количество интерфейсов, при условии, что эти интерфейсы `видимы` для использования классом. Однако, есть случаи, когда класс Zephir (и впоследствии расширение) может потребоваться для реализации интерфейса, который создан в другом расширении.

Например, если мы хотим реализовать `MiddlewareInterface` из `PSR` расширения, то нам нужно создать интерфейс-заглушку:

```zep
// middlewareinterfaceex.zep
namespace Test\Oo\Extend;

use Psr\Http\Server\MiddlewareInterface;

interface MiddlewareInterfaceEx extends MiddlewareInterface
{

}
```

С этого момента мы можем использовать интерфейс-заглушку в любом месте нашего расширения.

```php
/**
 * @test
 */
public function shouldExtendMiddlewareInterface()
{
    if (!extension_loaded('psr')) {
        $this->markTestSkipped(
            "Psr расширение не загружено"
        );
    }

    $this->assertTrue(
        is_subclass_of(MiddlewareInterfaceEx::class, 'Psr\Http\Server\MiddlewareInterface')
    );
}
```

**Примечание:** Ответственность разработчика — убедиться, что все внешние зависимости присутствуют до загрузки расширения. В примере выше, перед тем как Zephir расширение будет загружено **сначала** необходимо установить и включить расширение [PSR](https://pecl.php.net/package/psr).

<a name='implementing-methods'></a>

## Реализация методов
Ключевое слово `function` декларирует новый метод. Методы имеют те же модификаторы для разрешения видимости, что и PHP. Однако Zephir требует явно указывать модификаторы видимости:

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

Методы могут принимать как обязательные так и необязательные параметры:

```zep
namespace Test;

class MyClass
{

    /**
     * Все параметры обязательные
     */
    public function doSum1(a, b)
    {
        return a + b;
    }

    /**
     * Обязателен только 'a', 'b' не обязателен и имеет значение по умолчанию
     */
    public function doSum2(a, b = 3)
    {
        return a + b;
    }

    /**
     * Оба параметра не обязательны
     */
    public function doSum3(a = 1, b = 2)
    {
        return a + b;
    }

    /**
     * Параметры обязательны, и они должны быть целочисленными
     */
    public function doSum4(int a, int b)
    {
        return a + b;
    }

    /**
     * Статически типизированные параметры обязательны,
     * они целочисленны и имеют значения по умолчанию
     */
    public function doSum4(int a = 4, int b = 2)
    {
        return a + b;
    }
}
```

<a name='implementing-methods-optional-nullable-parameters'></a>

### Необязательные обнуляемые параметры
Zephir гарантирует, что значение переменной останется такого же типа, с которым она была объявлена. Это заставляет Zephir преобразовать значение `null` в ближайшее соответствующее типу значение:

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

### Поддерживаемые области видимости (инкапсуляция)
* Открытый (public): Методы, помеченные как `public`, экспортируются в PHP-расширение; это означает, что публичные методы доступны для PHP-кода, а также для самого расширения.

* Защищенный (protected): Методы, помеченные как `protected`, экспортируются в PHP-расширение; это означает, что защищенные методы доступны для PHP-кода, а также для самого расширения. Однако, защищенные методы могут быть вызваны либо в пределах класса, либо наследником класса.

* Закрытый (private): Методы, помеченные как `private`, не экспортируются в PHP-расширение; это означает, что приватные методы доступны только для класса, где они реализованы.

<a name='implementing-methods-supported-modifiers'></a>

### Поддерживаемые модификаторы
* Статический (`static`): Методы с этим модификатором могут вызываться только в статическом контексте (из класса, а не объекта).

* Финальный (`final`): Если метод имеет этот модификатор, он не может быть переопределён.

* Устаревший (`deprecated`): Методы, отмеченные как `deprecated` выбрасывают ошибку `E_DEPRECATED` в месте вызова.

<a name='implementing-methods-getter-setter-shortcuts'></a>

### Сокращения для геттеров и сеттеров
Как и в C#, в Zephir вы можете использовать `get`/`set`/`toString` сокращения. Эта особенность позволяет легко писать геттеры и сеттеры для свойств, без явной реализации этих методов как таковых.

Например, без сокращений нам нужен был бы такой код:

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

Вы можете реализовать ту же самую логику, используя сокращения, как показано ниже:

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

### Подсказки возвращаемого типа
Методы классов и интерфейсов могут иметь «подсказки возвращаемого типа». Это позволит компилятору предоставить полезную дополнительную информацию об ошибках в вашем приложении. Рассмотрим следующий пример:

```zep
namespace App;

class MyClass
{
    public function getSomeData() -> string
    {
        // здесь будет выброшено исключение времени компиляции
        // потому что возвращаемое значение (boolean)
        // не соответсвует ранее объявленному типу
        return false;
    }

    public function getSomeOther() -> <App\MyInterface>
    {
        // здесь будет выброшено исключение времени компиляции
        // если возвращаемый объект не реализует
        // ожидаемый компилитором интерфейс
        return new App\MyObject;
    }

    public function process()
    {
        var myObject;

        // подсказка типа сообщит компилятору, что
        // myObject является экземпляром класса,
        // который реализует App\MyInterface
        let myObject = this->getSomeOther();

        // компилятор проверит, декларирует ли интерфейс
        // App\MyInterface метод "someMethod"
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
Методы могут быть также помечены как `void`. Это значит, что метод не можете вернуть ничего:

```zep
public function setConnection(connection) -> void
{
    let this->_connection = connection;
}
```

Когда это может быть полезно? Потому что компилятор может определить, ожидает ли программа возврата значения из этих методов, и вызовет исключение компилятора:

```zep
let myDb = db->setConnection(connection); // тут сгенерируется исключение
myDb->execute("SELECT * FROM robots");
```

<a name='implementing-methods-strict-flexible-parameter-data-types'></a>

### Строгие и приводимые типы параметров
В Zephir вы можете указать тип данных каждого параметра метода. По умолчанию все типизированные аргументы приводимы. На практике это означает, что если значение не соответсвует ожидаемому типу (но является совместимым типом), Zephir прозрачно преобразует его в ожидаемый тип:

```zep
public function filterText(string text, boolean escape=false)
{
    //...
}
```

Метод выше будет работать в следующих ситуациях:

```zep
<?php

$o->filterText(1111, 1);              // OK
$o->filterText("some text", null);    // OK
$o->filterText(null, true);           // OK
$o->filterText("some text", true);    // OK
$o->filterText(array(1, 2, 3), true); // Ошибка
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

$o->filterText(1111, 1);              // Ошибка
$o->filterText("some text", null);    // OK
$o->filterText(null, true);           // Ошибка
$o->filterText("some text", true);    // OK
$o->filterText(array(1, 2, 3), true); // Ошибка
```

Указав какие параметры являются строгими, а какие приводимыми, разработчик может установить конкретное желаемое поведение.

<a name='implementing-methods-read-only-parameters'></a>

### Параметры только для чтения
Using the keyword `const` you can mark parameters as read-only, this helps to respect [const-correctness](https://en.wikipedia.org/wiki/Const_(computer_programming)). Параметры, помеченные этим атрибутом, не могут быть изменены внутри метода:

```zep
namespace App;

class MyClass
{
    // "a" только для чтения
    public function getSomeData(const string a)
    {
        // компилятор сгенерирует ошибку
        let a = "hello";
    }
}
```

Когда параметр объявлен только для чтения, компилятор может делать безопасные предположения и проводить дальнейшие оптимизации этих переменных.

<a name='implementing-properties'></a>

## Реализация свойств
Class member variables are called "properties". By default, they act the same as PHP properties. Properties are exported to the PHP extension, and are visible from PHP code. Properties implement the usual visibility modifiers available in PHP, and explicitly setting a visibility modifier is mandatory in Zephir:

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
Properties can be updated by accessing them using the `->` operator:

```zep
let this->myProperty = 100;
```

Zephir checks that properties exist when a program is accessing them. If a property is not declared, you will get a compiler exception:

```bash
CompilerException: Property '_optionsx' is not defined on class 'App\MyClass' in /Users/scott/utils/app/myclass.zep on line 62

      let this->_optionsx = options;
      ------------^
```

If you want to avoid this compiler validation, or just create a property dynamically, you can enclose the property name using brackets and string quotes:

```zep
let this->{"myProperty"} = 100;
```

You can also use a simple variable to update a property; the property name will be taken from the variable:

```zep
let someProperty = "myProperty";
let this->{someProperty} = 100;
```

<a name='implementing-properties-reading'></a>

## Чтение свойств
Properties can be read by accessing them using the `->` operator:

```zep
echo this->myProperty;
```

As when updating, properties can be dynamically read this way:

```zep
// Avoid compiler check or read a dynamic user defined property
echo this->{"myProperty"};

// Read using a variable name
let someProperty = "myProperty";
echo this->{someProperty}
```

<a name='class-constants'></a>

## Константы класса
Classes may contain class constants that remain the same and unchangeable once the extension is compiled. Class constants are exported to the PHP extension, allowing them to be used from PHP.

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
Methods can be called using the object operator `->` as in PHP:

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

Static methods must be called using the static operator `::`:

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

You can call methods in a dynamic manner as follows:

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
Zephir supports calling method parameters by name or keyword arguments. Named parameters can be useful if you want to pass parameters in an arbitrary order, document the meaning of parameters, or specify parameters in a more elegant way.

Consider the following example. A class called `Image` has a method that receives four parameters:

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

Using the standard method calling approach:

```zep
i->chop(100);             // width=100, height=400, x=0, y=0
i->chop(100, 50, 10, 20); // width=100, height=50, x=10, y=20
```

Using named parameters, you can:

```zep
i->chop(width: 100);              // width=100, height=400, x=0, y=0
i->chop(height: 200);             // width=600, height=200, x=0, y=0
i->chop(height: 200, width: 100); // width=100, height=200, x=0, y=0
i->chop(x: 20, y: 30);            // width=600, height=400, x=20, y=30
```

When the compiler (at compile time) does not know the correct order of these parameters, they must be resolved at runtime. In this case, there could be a minimum additional extra overhead:

```zep
let i = new {someClass}();
i->chop(y: 30, x: 20);
```    
