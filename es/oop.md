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

`final`: If a class has this modifier it cannot be extended:

```zep
namespace Test;

/**
 * Esta clase no puede se extendida por otras clases
 */
final class MyClass
{

}
```

`abstract: If a class has this modifier it cannot be instantiated:

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

The `function` keyword introduces a method. Los métodos implementan los modificadores de visibilidad generalmente disponibles en PHP. Establecer explícitamente un modificador de visibilidad es obligatorio en Zephir:

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

Zephir se asegura que el valor de una variable permanezca del tipo que fue declarada la variable. This makes Zephir convert the `null` value to the closest approximate value:

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

* Public: Methods marked as `public` are exported to the PHP extension; this means that public methods are visible to the PHP code as well to the extension itself.

* Protected: Methods marked as `protected` are exported to the PHP extension; this means that protected methods are visible to the PHP code as well to the extension itself. Sin embargo, sólo se pueden llamar estos métodos protegidos en el ámbito de la clase o en clases que la heredan.

* Private: Methods marked as `private` are not exported to the PHP extension; this means that private methods are only visible to the class where they're implemented.

<a name='implementing-methods-supported-modifiers'></a>

### Modificadores Soportados

* `static`: Methods with this modifier can only be called in a static context (from the class, not an object).

* `final`: If a method has this modifier it cannot be overriden.

* `deprecated`: Methods marked as `deprecated` throw an `E_DEPRECATED` error when they are called.

<a name='implementing-methods-getter-setter-shortcuts'></a>

### Métodos abreviados de getter/setter

Like in C#, you can use `get`/`set`/`toString` shortcuts in Zephir. Esta característica le permite escribir fácilmente setters y getters para las propiedades, sin implementar explícitamente los métodos como tales.

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

Un método puede tener más de un tipo de valor devuelto. When multiple types are defined, the operator `|` must be used to separate those types.

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

Los métodos también se pueden marcar como `void`. Esto significa que un método no puede devolver datos:

```zep
public function setConnection(connection) -> void
{
    let this->_connection = connection;
}
```

¿Por qué esto es útil? Porque el compilador puede detectar si el programa espera un valor devuelto de estos métodos y producir una excepción del compilador:

```zep
let myDb = db->setConnection(connection); // esto producirá una excepción
myDb->execute("SELECT * FROM robots");
```

<a name='implementing-methods-strict-flexible-parameter-data-types'></a>

### Tipos de datos de parámetro Estricto/Flexible

En Zephir, puede especificar el tipo de datos de cada parámetro de un método. Por defecto, estos tipos de datos son flexibles; esto significa que si se pasa un valor con un tipo de datos equivocado (pero compatible), Zephir intentará convertir de forma transparente al tipo esperado:

```zep
public function filterText(string text, boolean escape=false)
{
    //...
}
```

El método anterior funcionará con las siguientes llamadas:

```zep
<?php

$o->filterText(1111, 1);              // OK
$o->filterText("un texto", null);    // OK
$o->filterText(null, true);           // OK
$o->filterText("un texto", true);    // OK
$o->filterText(array(1, 2, 3), true); // FALLO
```

Sin embargo, pasando un tipo incorrecto, a menudo, podría conducir a errores. El uso incorrecto de una API específica produciría resultados inesperados. Puede no permitir la conversión automática del parámetro con un tipo de datos estricto:

```zep
public function filterText(string! text, boolean escape=false)
{
    //...
}
```

Ahora, la mayoría de las llamadas con un tipo incorrecto causarán una excepción debido a los tipos de datos no válidos pasados:

```zep
<?php

$o->filterText(1111, 1);              // FALLO
$o->filterText("un texto", null);    // OK
$o->filterText(null, true);           // FALLO
$o->filterText("un texto", true);    // OK
$o->filterText(array(1, 2, 3), true); // FALLO
```

Al especificar qué parámetros son estrictos y cuales pueden ser flexible, un desarrollador puede establecer el comportamiento específico que realmente desea.

<a name='implementing-methods-read-only-parameters'></a>

### Parámetros de sólo lectura

Utilizando la palabra clave `const` puede marcar los parámetros como de solo lectura, esto ayuda a respetar la [correctitud de constantes](http://en.wikipedia.org/wiki/Const-correctness). Los parámetros marcados con este atributo no pueden ser modificados dentro del método:

```zep
namespace App;

class MyClass
{
    // "a" es solo lectura
    public function getSomeData(const string a)
    {
        // esto arrojará una excepción del compilador
        let a = "hola";
    }
}
```

Cuando un parámetro es declarado como solo lectura, el compilador puede hacer suposiciones seguras y realizar futuras optimizaciones sobre estas variables.

<a name='implementing-properties'></a>

## Implementación de Propiedades

Las variables miembro de la clase se llaman "propiedades". Por defecto, estas actúan igual que las propiedades en PHP. Las propiedades se exportan a la extensión PHP y son accesibles desde el código PHP. Las propiedades implementan los modificadores de visibilidad generalmente disponibles en PHP, configurar explícitamente un modificador de visibilidad es obligatorio en Zephir:

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

Las propiedades pueden tener valores literales compatibles por defecto compatible. Estos valores deben poder evaluarse en tiempo de compilación y no deben depender de la información de tiempo de ejecución para poder ser evaluados:

```zep
namespace Test;

class MyClass
{

    protected myProperty1 = null;
    protected myProperty2 = false;
    protected myProperty3 = 2.0;
    protected myProperty4 = 5;
    protected myProperty5 = "mi valor";
}
```

<a name='implementing-properties-updating'></a>

## Actualización de propiedades

Properties can be updated by accessing them using the `->` operator:

```zep
let this->myProperty = 100;
```

Zephir comprueba estas propiedades existan cuando un programa está accediendo a ellas. Si no se declara una propiedad, se obtendrá una excepción del compilador:

```bash
CompilerException: Property '_optionsx' is not defined on class 'App\MyClass' in /Users/scott/utils/app/myclass.zep on line 62

      let this->_optionsx = options;
      ------------^
```

Si desea evitar esta validación del compilador, o crear una propiedad dinámicamente, puede incluir el nombre de la propiedad utilizando llaves y comillas dobles:

```zep
let this->{"myProperty"} = 100;
```

También puede utilizar una simple variable para actualizar una propiedad; se tomara el nombre de la variable:

```zep
let someProperty = "myProperty";
let this->{someProperty} = 100;
```

<a name='implementing-properties-reading'></a>

## Leyendo propiedades

Las propiedades pueden leerse accediendo a ellas mediante el operador `->`:

```zep
echo this->myProperty;
```

Como al actualizar, se puede leer dinámicamente de esta manera:

```zep
// Evitar comprobación del compilador o leer dinámicamente una propiedad definida por el usuario
echo this->{"myProperty"};

// Leer utilizando el nombre de la variable
let someProperty = "myProperty";
echo this->{someProperty}
```

<a name='class-constants'></a>

## Constante de Clase

Las clases pueden contener constantes de clase que siguen siendo las mismas e inmutables una vez compilada la extensión. Las constantes de clase son exportadas a la extensión PHP, lo que les permite ser utilizadas desde PHP.

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

## Llamando a métodos

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

Puede llamar a los métodos en una forma dinámica como la siguiente:

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

### Parámetros por Nombre

Zephir admite parámetros de método de llamada por nombre o argumentos de palabras clave. Los parámetros con nombre pueden ser útiles si desea pasar parámetros en un orden arbitrario, documentar el significado de los parámetros o especificar parámetros de una manera más elegante.

Considere el siguiente ejemplo. Una clase llamada `Image` tiene un método que recibe cuatro parámetros:

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

Utilizando el método estándar de llamado:

```zep
i->chop(100);             // width=100, height=400, x=0, y=0
i->chop(100, 50, 10, 20); // width=100, height=50, x=10, y=20
```

Utilizando parámetros nombrados, usted puede hacer lo siguiente:

```zep
i->chop(width: 100);              // width=100, height=400, x=0, y=0
i->chop(height: 200);             // width=600, height=200, x=0, y=0
i->chop(height: 200, width: 100); // width=100, height=200, x=0, y=0
i->chop(x: 20, y: 30);            // width=600, height=400, x=20, y=30
```

Cuando el compilador (en tiempo de compilación) no conoce el orden correcto de estos parámetros, estos deben resolverse en tiempo de ejecución. En este caso, podría haber una mínima sobrecarga adicional:

```zep
let i = new {someClass}();
i->chop(y: 30, x: 20);
```