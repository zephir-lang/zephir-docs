# Clases y Objetos

Zephir promueve la programación orientada a objetos. Por esta razón sólo puedes exportar métodos y clases en las extensiones. También verá que la mayoría de las veces, los errores de tiempo de ejecución arrojan excepciones en lugar de errores fatales o advertencias.

<a name='classes'></a>

## Clases

Cada archivo de Zephir debe implementar una clase o una interfaz (y sólo una). Una estructura de clase es muy similar a una clase en PHP:

```zep
namespace Test;

/**
 * Esta es una clase de ejemplo
 */
class MyClass
{

}
```

<a name='classes-modifiers'></a>

### Modificadores de Clase

Son soportadas los siguientes modificadores de clase:

*final*: Si una clase tiene este modificador no puede ser extendida:

```zep
namespace Test;

/**
 * Esta clase no puede se extendida por otras clases
 */
final class MyClass
{

}
```

*abstract*: Si una clase tiene este modificador no puede ser instanciada:

```zep
namespace Test;

/**
 * Esta clase no puede ser instanciada
 */
abstract class MyClass
{

}
```

<a name='classes-interfaces'></a>

### Implementación de Interfaces

Las clases de Zephir pueden implementar cualquier cantidad de interfaces, siempre que estas clases sean `visible` para que la clase las utilice. Sin embargo, hay ocasiones en que la clase Zephir (y posteriormente la extensión) puede requerir implementar una interfaz que se construya en una extensión diferente.

Si quieren implementar `MiddlewareInterface` de la extensión `PSR`, deberemos crear una interface `stub`:

```zep
// middlewareinterfaceex.zep
namespace Test\Oo\Extend;

use Psr\Http\Server\MiddlewareInterface;

interface MiddlewareInterfaceEx extends MiddlewareInterface
{

}
```

Desde aquí podemos utilizar la interfaz `stub` a lo largo de nuestra extensión.

```php
/**
 * @test
 */
public function shouldExtendMiddlewareInterface()
{
    if (!extension_loaded('psr')) {
        $this->markTestSkipped(
            "La extensión PSR no esta cargada"
        );
    }

    $this->assertTrue(
        is_subclass_of(MiddlewareInterfaceEx::class, 'Psr\Http\Server\MiddlewareInterface')
    );
}
```

**NOTA** Es responsabilidad del desarrollador asegurarse de que todas las referencias externas estén presentes antes de que se cargue la extensión. Entonces para el ejemplo anterior, la extensión [PSR](https://pecl.php.net/package/psr) tiene que estar cargada **antes** que la extensión construida en Zephir este cargada.

<a name='implementing-methods'></a>

## Implementación de Métodos

La palabra clave `function` introduce un método. Los métodos implementan los modificadores de visibilidad generalmente disponibles en PHP. Establecer explícitamente un modificador de visibilidad es obligatorio en Zephir:

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

Los métodos pueden recibir parámetros obligatorios y opcionales:

```zep
namespace Test;

class MyClass
{

    /**
     * Todos los parámetros son obligatorios
     */
    public function doSum1(a, b)
    {
        return a + b;
    }

    /**
     * Solo 'a' es obligatorio, 'b' es opcional y tiene un valor por defecto
     */
    public function doSum2(a, b = 3)
    {
        return a + b;
    }

    /**
     * Ambos parámetros son opcionales
     */
    public function doSum3(a = 1, b = 2)
    {
        return a + b;
    }

    /**
     * Los parámetros son obligatorios y sus valores deben ser del tipo integer
     */
    public function doSum4(int a, int b)
    {
        return a + b;
    }

    /**
     * Tipos estáticos con valores por defecto
     */
    public function doSum4(int a = 4, int b = 2)
    {
        return a + b;
    }
}
```

<a name='implementing-methods-optional-nullable-parameters'></a>

### Parámetros opcionales nulos

Zephir se asegura que el valor de una variable permanezca del tipo que fue declarada la variable. This makes Zephir convert the null value to the closest approximate value:

```zep
public function foo(int a = null)
{
    echo a; // si no es pasado "a" imprime 0
}

public function foo(boolean a = null)
{
    echo a; // si no es pasado "a" imprime false
}

public function foo(string a = null)
{
    echo a; // si no es pasado "a" imprime una cadena de texto vacía
}

public function foo(array a = null)
{
    var_dump(a); // si no es pasado "a" imprime un array vacío
}
```

<a name='implementing-methods-supported-visibilities'></a>

### Visibilidades soportadas

* Público: los métodos marcados como `public` se exportan a la extensión PHP; esto significa que métodos públicos son accesibles para el código PHP y para la extensión.

* Protegido: los métodos marcados como `protected` se exportan a la extensión PHP; esto significa que métodos protegidos son accesibles para el código PHP y para la extensión. Sin embargo, sólo se pueden llamar estos métodos protegidos en el ámbito de la clase o en clases que la heredan.

* Privado: los métodos marcados como `private` no se exportan a la extensión PHP; esto significa que métodos privados sólo son accesibles a la clase donde está implementados.

<a name='implementing-methods-supported-modifiers'></a>

### Modificadores Soportados

* Estáticos: los métodos con el modificador `static` sólo se pueden llamar en un contexto estático (de la clase, no de un objeto).

* Final: Si un método tiene el modificador `final` no puede ser sobre cargar.

* Obsoleto: los métodos que se marcan como `deprecated` arrojan un error `E_DEPRECATED` cuando se les llama.

<a name='implementing-methods-getter-setter-shortcuts'></a>

### Métodos abreviados de getter/setter

Como en C#, en Zephir puede utilizar métodos abreviados get/set/toString. Esta característica le permite escribir fácilmente setters y getters para las propiedades, sin implementar explícitamente los métodos como tales.

Por ejemplo, sin atajos necesitaríamos un código como el siguiente:

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

Es posible escribir el mismo código utilizando los siguiente métodos abreviados:

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

Cuando el código es compilado, estos métodos son exportados como métodos reales, pero usted no tiene que escribirlos manualmente.

<a name='implementing-methods-return-type-hints'></a>

### Tipo de valor devuelto

Los métodos en clases e interfaces pueden tener "sugerencias de tipo de retorno". Estos proporcionarán información adicional útil al compilador para informarle sobre los errores en su aplicación. Considere el siguiente ejemplo:

```zep
namespace App;

class MyClass
{
    public function getSomeData() -> string
    {
        // esto lanzará una excepción del compilador
        // ya que el valor devuelto (booleano) no coincide
        // la cadena de tipo devuelta esperada
        return false;
    }

    public function getSomeOther() -> <App\MyInterface>
    {
        // esto lanzará una excepción del compilador
        // si el objeto devuelto no implementa
        // la interfaz esperada App\MyInterface
        return new App\MyObject;
    }

    public function process()
    {
        var myObject;

        // la sugerencia de tipo le dirá al compilador que
        // myObject es una instancia de una clase
        // que implementa App\MyInterface
        let myObject = this->getSomeOther();

        // el compilador comprobará si App\MyInterface
        // implementa un método llamado "someMethod"
        echo myObject->someMethod();
    }
}
```

Un método puede tener más de un tipo de valor devuelto. Cuando se definen varios tipos, debe utilizarse el operador | para separar estos tipos.

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

### Tipo de valor devuelto: void

Methods can also be marked as `void`. This means that a method is not allowed to return any data:

```zep
public function setConnection(connection) -> void
{
    let this->_connection = connection;
}
```

Why is this useful? Because the compiler can detect if the program is expecting a return value from these methods, and produce a compiler exception:

```zep
let myDb = db->setConnection(connection); // this will produce an exception
myDb->execute("SELECT * FROM robots");
```

<a name='implementing-methods-strict-flexible-parameter-data-types'></a>

### Tipos de datos de parámetro Estricto/Flexible

In Zephir, you can specify the data type of each parameter of a method. By default, these data-types are flexible; this means that if a value with a wrong (but compatible) data-type is passed, Zephir will try to transparently convert it to the expected one:

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

However, passing a wrong type could often lead to bugs. Improper use of a specific API would produce unexpected results. You can disallow the automatic conversion by setting the parameter with a strict data-type:

```zep
public function filterText(string! text, boolean escape=false)
{
    //...
}
```

Now, most of the calls with a wrong type will cause an exception due to the invalid data types passed:

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

### Parámetros de sólo lectura

Using the keyword `const` you can mark parameters as read-only, this helps to respect [const-correctness](http://en.wikipedia.org/wiki/Const-correctness). Parameters marked with this attribute cannot be modified inside the method:

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

When a parameter is declared as read-only, the compiler can make safe assumptions and perform further optimizations over these variables.

<a name='implementing-properties'></a>

## Implementing Properties

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

Within class methods, non-static properties may be accessed by using -> (Object Operator):

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

## Updating Properties

Properties can be updated by accessing them using the '->' operator:

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

## Reading Properties

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

## Class Constants

Classes may contain class constants that remain the same and unchangeable once the extension is compiled. Class constants are exported to the PHP extension, allowing them to be used from PHP.

```zep
namespace Test;

class MyClass
{
    const MYCONSTANT1 = false;
    const MYCONSTANT2 = 1.0;
}
```

Class constants can be accessed using the class name and the static operator (::):

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

## Calling Methods

Methods can be called using the object operator (->) as in PHP:

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

Static methods must be called using the static operator (::):

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

### Parameters by Name

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