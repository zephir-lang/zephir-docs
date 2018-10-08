# Tipos

Zephir es dinámica y estáticamente tipificado. En este capítulo se destacan los tipos soportados y sus comportamientos.

<a name='dynamic-types'></a>

## Tipos Dinámicos

Las variables dinámicas son exactamente como en PHP. Pueden ser asignados y reasignados a diferentes tipos sin restricción.

Una variable dinámica debe declararse con la palabra clave `var`. El comportamiento es casi igual que en PHP:

    var a, b, c;
    

##### Inicializar las variables

    let a = "hola", b = false;
    

##### Cambiar sus valores

    let a = "hola", b = false;
    let a = 10, b = "140";
    

##### Realizar operaciones

    let c = a + b;
    

Pueden tener ocho tipos:

| Tipo             | Descripción                                                                                                             |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `array`          | Un array, también puede llamarse arreglo o matriz, es un mapa ordenado. Un mapa es un tipo que asocia valores a claves. |
| `boolean`        | Un valor booleano expresa un valor de verdad. Puede ser `true` o `false`                                                |
| `float`/`double` | Números de punto flotante. El tamaño de un flotante depende de la plataforma.                                           |
| `integer`        | Números enteros. El tamaño de un entero depende de la plataforma.                                                       |
| `null`           | El valor especial `NULL` representa que la variable no tiene valor.                                                     |
| `object`         | Abstracción de objectos como en PHP.                                                                                    |
| `resource`       | Un recurso contiene una referencia a un recurso externo.                                                                |
| `string`         | Es una cadena de texto o una serie de caracteres, donde un caracter es igual a un byte.                                 |

Revisar por más de estos tipos en el [manual de PHP](http://www.php.net/manual/en/language.types.php).

<a name='dynamic-types-arrays'></a>

### Arrays

The array implementation in Zephir is basically the same as in PHP: ordered maps optimized for several different uses; it can be treated as an array, list (vector), hash table (an implementation of a map), dictionary, collection, stack, queue, and probably more. As array values can be other arrays, trees and multidimensional arrays are also possible.

The syntax to define arrays is slightly different than in PHP:

##### Square braces must be used to define arrays

    let myArray = [1, 2, 3];
    

##### Double colon must be used to define hashes' keys

    let myHash = ["first": 1, "second": 2, "third": 3];
    

Only long and string values can be used as keys:

    let myHash = [0: "first", 1: true, 2: null];
    let myHash = ["first": 7.0, "second": "some string", "third": false];
    

<a name='dynamic-types-boolean'></a>

### Boolean

A boolean expresses a truth value. It can be either 'true' or 'false':

    var a = false, b = true;
    

<a name='dynamic-types-float-double'></a>

### Float/Double

Floating-point numbers (also known as "floats", "doubles", or "real numbers"). Floating-point literals are expressions with one or more digits, followed by a period (.), followed by one or more digits. The size of a float is platform-dependent, although a maximum of ~1.8e308 with a precision of roughly 14 decimal digits is a common value (the 64 bit IEEE format).

    var number = 5.0, b = 0.014;
    

Floating point numbers have limited precision. Although it depends on the system, Zephir uses the same IEEE 754 double precision format used by PHP, which will give a maximum relative error due to rounding in the order of 1.11e-16.

<a name='dynamic-types-integer'></a>

### Integer

Números enteros. The size of an integer is platform-dependent, although a maximum value of about two billion is the usual value (that's 32 bits signed). 64-bit platforms usually have a maximum value of about 9E18. PHP does not support unsigned integers so Zephir has this restriction too:

    var a = 5, b = 10050;
    

<a name='dynamic-types-integer-overflow'></a>

### Integer sobrecarga

Contrary to PHP, Zephir does not automatically check for integer overflows. Like in C, if you are doing operations that may return a big number, you should use types such as 'unsigned long' or 'float' to store them:

    unsigned long my_number = 2147483648;
    

<a name='dynamic-types-objects'></a>

### Objects

Zephir allows to instantiate, manipulate, call methods, read class constants, etc from PHP objects:

    let myObject = new stdClass(),
        myObject->someProperty = "my value";
    

<a name='dynamic-types-string'></a>

### String

A string is series of characters, where a character is the same as a byte. As PHP, Zephir only supports a 256-character set, and hence does not offer native Unicode support.

    var today = "friday";
    

In Zephir, string literals can only be specified using double quotes (like in C or Go). Single quotes are reserved for chars.

The following escape sequences are supported in strings:

| Sequence    | Description     |
| ----------- | --------------- |
| `\\t`     | Horizontal tab  |
| `\\n`     | Line feed       |
| `\\r`     | Carriage return |
| `\\ \` | Backslash       |
| `\\"`     | double-quote    |

    var today    = "\tfriday\n\r",
        tomorrow = "\tsaturday";
    

In Zephir, strings don't support variable parsing like in PHP; you need to use concatenation instead:

    var name = "peter";
    
    echo "hello: " . name;
    

<a name='static-types'></a>

## Static Types

Static typing allows the developer to declare and use some variable types available in C. Variables can't change their type once they're declared as static types. However, they allow the compiler to do a better optimization job. The following types are supported:

| Tipo               | Description                                                                    |
| ------------------ | ------------------------------------------------------------------------------ |
| `array`            | A structure that can be used as hash, map, dictionary, collection, stack, etc. |
| `boolean`          | A boolean expresses a truth value. It can be either 'true' or 'false'.         |
| `char`             | Smallest addressable unit of the machine that can contain basic character set. |
| `float`/`double`   | Double precision floating-point type. The size is platform-dependent.          |
| `integer`          | Signed integers. At least 16 bits in size.                                     |
| `long`             | Long signed integer type. At least 32 bits in size.                            |
| `string`           | A string is a series of characters, where a character is the same as a byte.   |
| `unsigned char`    | Same size as char, but guaranteed to be unsigned.                              |
| `unsigned integer` | Unsigned integers. At least 16 bits in size.                                   |
| `unsigned long`    | Same as long, but unsigned.                                                    |

<a name='static-types-boolean'></a>

### Boolean

A boolean expresses a truth value. It can be either 'true' or 'false'. Contrary to the dynamic behavior detailed above, static boolean types remain boolean (true or false) no mater what value is assigned to them:

    boolean a;
    let a = true;
    

##### automatically casted to `true`

    let a = 100;
    

##### automatically casted to `false`

##### throws a compiler exception

    let a = "hello";
    

<a name='static-types-char-unsigned'></a>

### Char/Char sin signo

Char variables are the smallest addressable unit of the machine that can contain the basic character set (generally 8 bits). A 'char' variable can be used to store any character in a string:

    char ch, string name = "peter";
    

##### stores 't'

    let ch = name[2];
    

##### `char` literals must be enclosed in single quotes

    let ch = 'Z';
    

<a name='static-types-integer-unsigned'></a>

### Integer/Integer sin signo

Integer values are like the integer member in dynamic values. Values assigned to integer variables remain integer:

    int a;
    
    let a = 50,
        a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### throws a compiler exception

    let a = "hello";
    

Unsigned integers are like integers but they don't have sign, this means you can't store negative numbers in these sort of variables:

    uint a;
    
    let a = 50;
    

##### automatically casted to 70

    let a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### throws a compiler exception

    let a = "hello";
    

Unsigned integers are twice bigger than standard integers. Assigning unsigned integers to standard (signed) integers may result in loss of data:

##### potential loss of data for `b`

    uint a, int b;
    
    let a = 2147483648,
        b = a;
    

<a name='static-types-long-unsigned'></a>

### Long/Long sin signo

Long variables are twice bigger than integer variables, thus they can store bigger numbers. As with integers, values assigned to long variables are automatically casted to this type:

    long a;
    
    let a = 50,
        a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### throws a compiler exception

    let a = "hello";
    

Unsigned longs are like longs but they are not signed, this means you can't store negative numbers in these sort of variables:

    ulong a;
    
    let a = 50;
    

##### automatically casted to 70

    let  a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### throws a compiler exception

    let a = "hello";
    

Unsigned longs are twice bigger than standard longs; assigning unsigned longs to standard (signed) longs may result in loss of data:

##### potential loss of data for `b`

    ulong a, long b;
    
    let a = 4294967296,
        b = a;
    

<a name='static-types-string'></a>

### String

A string is series of characters, where a character is the same as a byte. As in PHP it only supports a 256-character set, and hence does not offer native Unicode support.

When a variable is declared string it never changes its type:

    string a;
    
    let a = "";
    

##### string literals must be enclosed in double quotes

    let  a = "hello";
    

##### converted to string "A"

    let a = 'A';
    

##### automatically casted to ""

    let a = null;