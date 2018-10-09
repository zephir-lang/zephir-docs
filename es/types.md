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

La implementación de los arreglos en Zephir es básicamente igual que en PHP: Mapas ordenados optimizados para varios usos diferentes; se puede tratar como un array, lista (vector), tabla hash (una implementación de un mapa), diccionario, colección, pila, cola y probablemente más. Como valores del array pueden ser otros arrays, árboles y también son posibles arrays multidimensionales.

La sintaxis para definir matrices es ligeramente diferente a PHP:

##### Deben utilizarse corchetes para definir matrices

    let myArray = [1, 2, 3];
    

##### Deben utilizarse dos puntos ":" para definir claves hash

    let myHash = ["first": 1, "second": 2, "third": 3];
    

Solo valores enteros y cadenas de textos pueden utilizados como claves:

    let myHash = [0: "primero", 1: true, 2: null];
    let myHash = ["primero": 7.0, "segundo": "una cadena", "tercero": false];
    

<a name='dynamic-types-boolean'></a>

### Boolean

Un valor booleano expresa un valor de verdad. Puede ser `true` o `false`:

    var a = false, b = true;
    

<a name='dynamic-types-float-double'></a>

### Float/Double

Los números de punto flotante (también conocido como "flotantes", "dobles" o "números reales"). Los literales de coma flotante son expresiones con uno o más dígitos, seguidos de un punto (.), seguido por uno o más dígitos. El tamaño de un float es dependiente de la plataforma, aunque un tamaño máximo de ~1.8e308 con una precisión de aproximadamente 14 dígitos decimales es un valor común (el formato IEEE de 64 bits).

    var number = 5.0, b = 0.014;
    

En los números de coma flotante tienen una precisión limitada. Aunque depende del sistema, Zephir utiliza el mismo formato de doble precisión de IEEE 754 usado por PHP, que le dará un máximo error relativo por redondeo en el orden de 1.11e-16.

<a name='dynamic-types-integer'></a>

### Integer

Números enteros. El tamaño de un entero es dependiente de la plataforma, aunque un valor máximo aproximadamente de 2 billones es el valor usual (que es 32 bits firmados). Las plataformas de 64 bits generalmente tienen un valor máximo aproximadamente de 9E18. PHP no soporta enteros sin signo, Zephir también tiene esta restricción:

    var a = 5, b = 10050;
    

<a name='dynamic-types-integer-overflow'></a>

### Integer sobrecarga

Contrario a PHP, Zephir no comprueba automáticamente el desborde de enteros. Como en C, si haces operaciones que pueden devolver un número grande, usted debe usar tipos como 'unsigned long' o 'float' para almacenarlos:

    unsigned long my_number = 2147483648;
    

<a name='dynamic-types-objects'></a>

### Objects

Zephir permite crear, manipular, llamar métodos, leer constantes de clase, etcétera desde objetos PHP:

    let myObject = new stdClass(),
        myObject->someProperty = "mi valor";
    

<a name='dynamic-types-string'></a>

### String

Es una cadena de texto o una serie de caracteres, donde un caracter es igual a un byte. Como en PHP, Zephir solo soporta un conjunto de 256 caracteres y por lo tanto no ofrece un soporte nativo de Unicode.

    var today = "Viernes";
    

En Zephir, los string literales solo pueden ser especificados utilizando las comillas dobles (como en C o Go). Las comillas simples están reservadas para los caracteres.

Son soportadas las siguientes secuencias de escape en strings:

| Secuencia   | Descripción      |
| ----------- | ---------------- |
| `\\t`     | Tab horizontal   |
| `\\n`     | Salto de línea   |
| `\\r`     | Retorno de carro |
| `\\ \` | Barra invertida  |
| `\\"`     | Comilla doble    |

    var today    = "\tviernes\n\r",
        tomorrow = "\tsábado";
    

En Zephir, los strings no soportan el analizis de variables como en PHP; es necesario utilizar la concatenación:

    var name = "Pedro";
    
    echo "hola: " . name;
    

<a name='static-types'></a>

## Tipos Estáticos

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

    let a = "hola";
    

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

    let a = "hola";
    

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

    let a = "hola";
    

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

    let a = "hola";
    

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

    let a = "hola";
    

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