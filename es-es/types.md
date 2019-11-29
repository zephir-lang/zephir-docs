---
layout: default
language: 'en'
version: '0.12'
---

# Tipos

Zephir es dinámica y estáticamente tipificado. En este capítulo se destacan los tipos soportados y sus comportamientos.

<a name='dynamic-types'></a>

## Tipos Dinámicos

Las variables dinámicas son exactamente como en PHP. Pueden ser asignados y reasignados a diferentes tipos sin restricción.

Una variable dinámica debe declararse con la palabra clave `var`. El comportamiento es casi igual que en PHP:

```zephir
var a, b, c;
```

##### Inicializar las variables

```zephir
let a = "hola", b = false;
```

##### Cambiar sus valores

```zephir
let a = "hola", b = false;
let a = 10, b = "140";
```

##### Realizar operaciones

```zephir
let c = a + b;
```

Pueden tener ocho tipos:

| Tipo             | Descripción                                                                                                             |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `array`          | Un array, también puede llamarse arreglo o matriz, es un mapa ordenado. Un mapa es un tipo que asocia valores a claves. |
| `boolean`        | Un valor booleano expresa un valor de verdad. Puede ser `true` o `false`.                                               |
| `float`/`double` | Son números de punto flotante. El tamaño de un flotante depende de la plataforma.                                       |
| `integer`        | Son números enteros. El tamaño de un entero depende de la plataforma.                                                   |
| `null`           | El valor especial `NULL` representa que la variable no tiene valor.                                                     |
| `object`         | Abstracción de objectos como en PHP.                                                                                    |
| `resource`       | Un recurso contiene una referencia a un recurso externo.                                                                |
| `string`         | Es una cadena de texto o una serie de caracteres, donde un caracter es igual a un byte.                                 |


Revisar por más de estos tipos en el [manual de PHP](http://www.php.net/manual/en/language.types.php).

<a name='dynamic-types-arrays'></a>

### Array

La implementación de los arreglos en Zephir es básicamente igual que en PHP: Mapas ordenados optimizados para varios usos diferentes; se puede tratar como un array, lista (vector), tabla hash (una implementación de un mapa), diccionario, colección, pila, cola y probablemente más. Como valores del array pueden ser otros arrays, árboles y también son posibles arrays multidimensionales.

La sintaxis para definir matrices es ligeramente diferente a PHP:

##### Deben utilizarse corchetes para definir matrices

```zephir
let myArray = [1, 2, 3];
```

##### Deben utilizarse dos puntos ":" para definir claves hash

```zephir
let myHash = ["first": 1, "second": 2, "third": 3];
```

Solo valores enteros y cadenas de textos pueden utilizados como claves:

```zephir
let myHash = [0: "primero", 1: true, 2: null];
let myHash = ["primero": 7.0, "segundo": "una cadena", "tercero": false];
```

<a name='dynamic-types-boolean'></a>

### Boolean

Un valor booleano expresa un valor de verdad. Puede ser `true` o `false`:

```zephir
var a = false, b = true;
```

<a name='dynamic-types-float-double'></a>

### Float/Double

Los números de punto flotante (también conocido como "flotantes", "dobles" o "números reales"). Los literales de coma flotante son expresiones con uno o más dígitos, seguidos de un punto (.), seguido por uno o más dígitos. El tamaño de un flotante es dependiente de la plataforma, aunque un tamaño máximo de ~1.8e308 con una precisión de aproximadamente 14 dígitos decimales es un valor común (el formato IEEE de 64 bits).

```zephir
var number = 5.0, b = 0.014;
```

Los números de punto flotante tienen una precisión limitada. Aunque depende del sistema, Zephir utiliza el mismo formato de doble precisión de IEEE 754 usado por PHP, que le dará un máximo error relativo por redondeo en el orden de 1.11e-16.

<a name='dynamic-types-integer'></a>

### Integer

Son números enteros. El tamaño de un entero es dependiente de la plataforma, aunque un valor máximo aproximadamente de 2 billones es el valor usual (que es 32 bits con signo). Las plataformas de 64 bits generalmente tienen un valor máximo aproximadamente de 9E18. PHP no soporta enteros sin signo, Zephir también tiene esta restricción:

```zephir
var a = 5, b = 10050;
```

<a name='dynamic-types-integer-overflow'></a>

### Integer sobrecarga

Contrario a PHP, Zephir no comprueba automáticamente el desborde de enteros. Como en C, si haces operaciones que pueden devolver un número grande, usted debe usar tipos como `unsigned long` o `float` para almacenarlos:

```zephir
unsigned long my_number = 2147483648;
```

<a name='dynamic-types-objects'></a>

### Object

Zephir permite crear, manipular, llamar métodos, leer constantes de clase, etcétera desde objetos PHP:

```zephir
let myObject = new \stdClass(),
    myObject->someProperty = "mi valor";
```

<a name='dynamic-types-string'></a>

### String

Un `string` es una serie de caracteres, donde un caracter es igual a un byte. Como en PHP, Zephir solo soporta un conjunto de 256 caracteres y por lo tanto no ofrece un soporte nativo de Unicode.

```zephir
var today = "Viernes";
```

En Zephir, los string literales solo pueden ser especificados utilizando las comillas dobles (como en C o Go). Las comillas simples están reservadas para los datos de tipo `char`.

Son soportadas las siguientes secuencias de escape en cadenas de texto:

| Secuencia | Descripción      |
| --------- | ---------------- |
| `\t`     | Tab horizontal   |
| `\n`     | Salto de línea   |
| `\r`     | Retorno de carro |
| `\`    | Barra invertida  |
| `\"`     | Comilla doble    |


```zephir
var today    = "\tviernes\n\r",
    tomorrow = "\tsábado";
```

En Zephir, los strings no soportan el analizis de variables como en PHP; es necesario utilizar la concatenación:

```zephir
var name = "Pedro";

echo "hola: " . name;
```

<a name='static-types'></a>

## Tipos Estáticos

El tipificado estático permite al programador declarar y utilizar algunos tipos de variables disponibles en C. Las variables no pueden cambiar su tipo una vez que se han declarado con un tipo estático. Sin embargo, permiten al compilador hacer un mejor trabajo de optimización. Son soportados los siguientes tipos:

| Tipo               | Descripción                                                                                     |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| `array`            | Una estructura que puede ser utilizada como hash, mapa, diccionario, colección, pila, etcétera. |
| `boolean`          | Un valor booleano expresa un valor de verdad. Puede ser `true` o `false`.                       |
| `char`             | Es la unidad más pequeña que puede contener el conjunto de caracteres básicos.                  |
| `float`/`double`   | Tipo punto flotante de doble precisión. El tamaño depende de la plataforma.                     |
| `integer`          | Enteros con signo. Al menos 16 bits de tamaño.                                                  |
| `long`             | Tipo entero largo con signo. Al menos 32 bits de tamaño.                                        |
| `string`           | Es una cadena de texto o una serie de caracteres, donde un caracter es igual a un byte.         |
| `unsigned char`    | Mismo tamaño que un `char`, pero garantiza que sea sin signo.                                   |
| `unsigned integer` | Enteros sin signo. Al menos 16 bits de tamaño.                                                  |
| `unsigned long`    | Igual que `long`, pero sin signo.                                                               |

<a name='static-types-boolean'></a>

### Boolean

Un `boolean` expresa un valor de verdad. Puede ser `true` o `false`. Contrariamente al comportamiento dinámico detallado anteriormente, los tipos `boolean` estáticos siguen siendo `boolean` (`true` o `false`) sin importar el valor que se les asigne:

```zephir
boolean a;
let a = true;
```

##### Automáticamente clasificado como `true`

```zephir
let a = 100;
```

##### Automáticamente clasificado como `false`

```zephir
let a = 0;
```

##### Arroja una excepción de compilador

```zephir
let a = "hola";
```

<a name='static-types-char-unsigned'></a>

### Char/Char sin signo

las variables `char` son la unidad direccionable más pequeña de la máquina que puede contener el conjunto de carácter básico (generalmente 8 bits). Una variable de tipo `char` se puede utilizar para almacenar cualquier carácter en una cadena:

```zephir
char ch, string name = "pedro";
```

##### almacenar 'd'

```zephir
let ch = name[2];
```

##### Los literales `char` deben ser encerrados entre comillas simples

```zephir
let ch = 'Z';
```

<a name='static-types-integer-unsigned'></a>

### Integer/Integer sin signo

`integer` values are like the `integer` member in dynamic values. Values assigned to integer variables remain integer:

```zephir
int a;

let a = 50,
    a = -70;
```

##### Automáticamente clasificado a 100

```zephir
let a = 100.25;
```

##### Automáticamente clasificado a 0

```zephir
let a = null;
```

##### Automáticamente clasificado a 0

```zephir
let a = false;
```

##### Arroja una excepción de compilador

```zephir
let a = "hola";
```

Los `unsigned integer`, son como los `integer` pero no tienen signo, esto significa que no puede almacenar números negativos en este tipo de variables:

```zephir
uint a;

let a = 50;
```

##### Automáticamente clasificado a 70

```zephir
let a = -70;
```

##### Automáticamente clasificado a 100

```zephir
let a = 100.25;
```

##### Automáticamente clasificado a 0

```zephir
let a = null;
```

##### Automáticamente clasificado a 0

```zephir
let a = false;
```

##### Arroja una excepción de compilador

```zephir
let a = "hola";
```

Las variables `unsigned integer` son dos veces más grandes que las variables `integer` comunes. Asignando un `unsigned integer` a un `integer` (sin signo) puede resultar en perdida de datos:

##### Pérdida potencial de datos en `b`

```zephir
uint a, int b;

let a = 2147483648,
    b = a;
```

<a name='static-types-long-unsigned'></a>

### Long/Long sin signo

Las variables `long` son dos veces más grandes que las variables `integer`, por lo que pueden almacenar números más grandes. Al igual que con los `integer`, los valores asignados a variables `long` se convierten automáticamente a este tipo:

```zephir
long a;

let a = 50,
    a = -70;
```

##### Automáticamente clasificado a 100

```zephir
let a = 100.25;
```

##### Automáticamente clasificado a 0

```zephir
    let a = null;
```

##### Automáticamente clasificado a 0

```zephir
let a = false;
```

##### Arroja una excepción de compilador

```zephir
let a = "hola";
```

Los `unsigned long` son como los `long` estándar, pero no tienen signo, esto significa que no puede almacenar números negativos en este tipo de variables:

```zephir
ulong a;

let a = 50;
```

##### Automáticamente clasificado a 70

```zephir
let  a = -70;
```

##### Automáticamente clasificado a 100

```zephir
let a = 100.25;
```

##### Automáticamente clasificado a 0

```zephir
let a = null;
```

##### Automáticamente clasificado a 0

```zephir
let a = false;
```

##### Arroja una excepción de compilador

```zephir
let a = "hola";
```

Las variables `unsigned long` son dos veces más grandes que las `long` estándar; asignando `unsigned long` a una estándar `long` (con signo) puede causar la perdida de datos:

##### Pérdida potencial de datos en `b`

```zephir
ulong a, long b;

let a = 4294967296,
    b = a;
```

<a name='static-types-string'></a>

### String

Es una cadena de texto o una serie de caracteres, donde un caracter es igual a un byte. Como PHP solo soporta un conjunto de 256 caracteres y por lo tanto no ofrece un soporte nativo de Unicode.

Cuando una variable es declarada como `string`, nunca cambia su tipo:

```zephir
string a;

let a = "";
```

##### Los string literales deben ser encerrados entre comillas dobles

```zephir
let a = "hola";
```

##### Convertido a string "A"

```zephir
let a = 'A';
```

##### Automáticamente clasificado a ""

```zephir
let a = null;
```