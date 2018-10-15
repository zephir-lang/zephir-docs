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

El tipificado estático permite al programador a declarar y utilizar algunos tipos de variables disponible en C. Las variables no pueden cambiar su tipo una vez que se han declarado con un tipo estático. Sin embargo, permiten al compilador hacer un mejor trabajo de optimización. Son soportados los siguientes tipos:

| Tipo               | Descripción                                                                                     |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| `array`            | Una estructura que puede ser utilizada como hash, mapa, diccionario, colección, pila, etcétera. |
| `boolean`          | Un valor booleano expresa un valor de verdad. Puede ser `true` o `false`.                       |
| `char`             | Es la unidad más pequeña que puede contener el conjunto de caracteres básicos.                  |
| `float`/`double`   | Tipo punto flotante de doble precisión. El tamaño depende de la plataforma.                     |
| `integer`          | Enteros con signo. Al menos 16 bits de tamaño.                                                  |
| `long`             | Tipo entero largo con signo. Al menos 32 bits de tamaño.                                        |
| `string`           | Es una cadena de texto o una serie de caracteres, donde un caracter es igual a un byte.         |
| `unsigned char`    | Mismo tamaño que un char, pero garantiza que sea sin signo.                                     |
| `unsigned integer` | Enteros sin signo. Al menos 16 bits de tamaño.                                                  |
| `unsigned long`    | Al igual que el tipo long, pero sin signo.                                                      |

<a name='static-types-boolean'></a>

### Boolean

Un valor booleano expresa un valor de verdad. Puede ser `true` o `false`. Contrariamente al comportamiento dinámico detallado anteriormente, los tipos booleanos estáticos siguen siendo booleanos (verdaderos o falsos) sin importar el valor que se les asigne:

    boolean a;
    let a = true;
    

##### Automáticamente clasificado como `true`

    let a = 100;
    

##### Automáticamente clasificado como `false`

##### Arroja una excepción de compilador

    let a = "hola";
    

<a name='static-types-char-unsigned'></a>

### Char/Char sin signo

Las variables char son la unidad direccionable más pequeña de la máquina que puede contener el conjunto de carácter básico (generalmente 8 bits). Una variable de 'tipo char' se puede utilizar para almacenar cualquier carácter en una cadena:

    char ch, string name = "pedro";
    

##### almacenar 'd'

    let ch = name[2];
    

##### Los literales `char` deben ser encerrados entre comillas simples

    let ch = 'Z';
    

<a name='static-types-integer-unsigned'></a>

### Integer/Integer sin signo

Los valores enteros el miembro entero en valores dinámicos. Los valores asignados a las variables de número entero permanecen enteros:

    int a;
    
    let a = 50,
        a = -70;
    

##### Automáticamente clasificado a 100

    let a = 100.25;
    

##### Automáticamente clasificado a 0

    let a = null;
    

##### Automáticamente clasificado a 0

    let a = false;
    

##### Arroja una excepción de compilador

    let a = "hola";
    

Los enteros sin signo, son como los enteros pero no tienen signo, esto significa que no puede almacenar números negativos en este tipo de variables:

    uint a;
    
    let a = 50;
    

##### Automáticamente clasificado a 70

    let a = -70;
    

##### Automáticamente clasificado a 100

    let a = 100.25;
    

##### Automáticamente clasificado a 0

    let a = null;
    

##### Automáticamente clasificado a 0

    let a = false;
    

##### Arroja una excepción de compilador

    let a = "hola";
    

Los enteros sin signo son dos veces más grandes que los enteros estándar. La asignación de enteros sin signo a enteros estándar (con signo) puede resultar en pérdida de datos:

##### Pérdida potencial de datos en `b`

    uint a, int b;
    
    let a = 2147483648,
        b = a;
    

<a name='static-types-long-unsigned'></a>

### Long/Long sin signo

Las variables long son dos veces más grandes que las variables enteras, por lo que pueden almacenar números más grandes. Al igual que con los enteros, los valores asignados a variables largas se convierten automáticamente a este tipo:

    long a;
    
    let a = 50,
        a = -70;
    

##### Automáticamente clasificado a 100

    let a = 100.25;
    

##### Automáticamente clasificado a 0

    let a = null;
    

##### Automáticamente clasificado a 0

    let a = false;
    

##### Arroja una excepción de compilador

    let a = "hola";
    

Los longs sin signo son como los longs estandar, pero no tienen signo, esto significa que no puede almacenar números negativos en este tipo de variables:

    ulong a;
    
    let a = 50;
    

##### Automáticamente clasificado a 70

    let  a = -70;
    

##### Automáticamente clasificado a 100

    let a = 100.25;
    

##### Automáticamente clasificado a 0

    let a = null;
    

##### Automáticamente clasificado a 0

    let a = false;
    

##### Arroja una excepción de compilador

    let a = "hola";
    

Los largos sin signo son dos veces más grandes que los longs estándar; la asignación de largos sin firmar a largos estándar (con signo) puede provocar la pérdida de datos:

##### Pérdida potencial de datos en `b`

    ulong a, long b;
    
    let a = 4294967296,
        b = a;
    

<a name='static-types-string'></a>

### String

Es una cadena de texto o una serie de caracteres, donde un caracter es igual a un byte. Como PHP solo soporta un conjunto de 256 caracteres y por lo tanto no ofrece un soporte nativo de Unicode.

Cuando una variable es declarada como string, nunca cambiar su tipo:

    string a;
    
    let a = "";
    

##### Los string literales deben ser encerrados entre comillas dobles

    let a = "hola";
    

##### Convertido a string "A"

    let a = 'A';
    

##### Automáticamente clasificado a ""

    let a = null;