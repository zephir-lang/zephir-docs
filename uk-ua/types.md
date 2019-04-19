* * *

layout: default language: 'uk' version: '0.10'

* * *

# Types

Zephir поєднує в собі статичну та динамічну типізацію. У цьому розділі ми розглянемо підтримувані типи даних та їхню поведінку.

<a name='dynamic-types'></a>

## Dynamic Types

Динамічних змінні працюють так само, як і в PHP. Їм можна призначати та перепризначати значення різних типів без обмежень.

Динамічна змінна має бути оголошеною з ключовим словом `var`. Поведінка майже така сама, як і в PHP:

```zephir
var a, b, c;
```

##### Ініціалізація змінних

```zephir
let a = "hello", b = false;
```

##### Зміна їхнього значення

```zephir
let a = "hello", b = false;
let a = 10, b = "140";
```

##### Виконання операцій

```zephir
let c = a + b;
```

Вони можуть бути восьми типів:

| Type             | Description                                                                 |
| ---------------- | --------------------------------------------------------------------------- |
| `array`          | An array is an ordered map. A map is a type that associates values to keys. |
| `boolean`        | A boolean expresses a truth value. Він може бути `true` або `false`.        |
| `float`/`double` | Floating point numbers. The size of a float is platform-dependent.          |
| `integer`        | Integer numbers. The size of an integer is platform-dependent.              |
| `null`           | The special NULL value represents a variable with no value.                 |
| `object`         | Object abstraction like in PHP.                                             |
| `resource`       | Ресурс містить посилання на зовнішній ресурс.                               |
| `string`         | A string is series of characters, where a character is the same as a byte.  |

Більше про типи ви можете дізнатися в [Документації PHP](http://www.php.net/manual/en/language.types.php).

<a name='dynamic-types-arrays'></a>

### Array

Реалізація масивів у Zephir в основному така сама як у PHP: впорядковані мапи оптимізовані для деяких випадків; можна розглядати як масив список (вектор) хеш-таблицю (реалізація мапи), словник, колекція, стек, черги. Можливі й інші трактування. Значеннями масиву можуть бути інші масиви, дерева, та багатовимірні масиви.

Синтаксис оголошення масиву дещо відрізняється від PHP:

##### Square braces must be used to define arrays

```zephir
let myArray = [1, 2, 3];
```

##### Double colon must be used to define hashes' keys

```zephir
let myHash = ["first": 1, "second": 2, "third": 3];
```

Ключами масиву можуть лише цілі числа та рядки:

```zephir
let myHash = [0: "first", 1: true, 2: null];
let myHash = ["first": 7.0, "second": "some string", "third": false];
```

<a name='dynamic-types-boolean'></a>

### Boolean

A boolean expresses a truth value. Він може бути `true` або `false`:

```zephir
var a = false, b = true;
```

<a name='dynamic-types-float-double'></a>

### Float/Double

Числа з рухомою комою (також відомі як "двійкові", "числа з подвійною точністю", "дійсні числа"). Літерали з рухомою комою - це вирази з однією або кількома цифрами, а потім - крапка (.), після нього одна або кілька цифр. Розмір числа після крапки залежить від платформи, хоча максимум ~1.8e308 з точністю приблизно 14 десяткових цифр мають загальне значення (64-бітний форматі IEEE).

```zephir
var number = 5.0, b = 0.014;
```

Числа з рухомою комою мають обмежену точність. Хоча це залежить від системи, як і в PHP, Zephir використовує формат подвійної точності IEEE 754, який дасть максимальну відносну помилку через округлення порядку 1.11e-16.

<a name='dynamic-types-integer'></a>

### Integer

Integer numbers. Розмір числа залежить від платформи, хоча максимальне значення для 32-бітного знакового числа є 2,147,483,647. 64-розрядні платформи зазвичай мають максимальне значення близько 9E18. PHP не підтримує цілі числа без знаку, тому Zephir теж має це обмеження:

```zephir
var a = 5, b = 10050;
```

<a name='dynamic-types-integer-overflow'></a>

### Integer overflow

На відміну від PHP Zephir автоматично не перевіряє рівень заповненості цілого числа. Як і в C, якщо ви виконуєте операції які можуть повернути велике число ви повинні використати такі типи як `unsigned long` або ж `float`:

```zephir
unsigned long my_number = 2147483648;
```

<a name='dynamic-types-objects'></a>

### Object

Zephir дозволяє створювати екземпляри PHP класів, маніпулювати PHP-об'єктами, викликати методи, читати константи класу та інші речі, які дозволяють PHP-об'єкти:

```zephir
let myObject = new \stdClass(),
    myObject->someProperty = "my value";
```

<a name='dynamic-types-string'></a>

### String

Рядок `string` є послідовністю символів, де кожен символ є одним байтом. Як і PHP, Zephir підтримує лише 256-символьний набір, а отже не дає вбудованої підтримки Unicode.

```zephir
var today = "friday";
```

У Zephir рядкові літерали можна задавати лише взявши їх у подвійні лапки (як у C або Go). Одинарні лапки зарезервовані для типу даних `char`.

У рядках підтримуються наступні символи екранування:

| Sequence | Description     |
| -------- | --------------- |
| `\t`    | Horizontal tab  |
| `\n`    | Line feed       |
| `\r`    | Carriage return |
| `\`   | Backslash       |
| `\"`    | double-quote    |

```zephir
var today    = "\tfriday\n\r",
    tomorrow = "\tsaturday";
```

Zephir не підтримує інтерполяцію змінних як це було в PHP; натомість ви повинні використовувати конкатенацію:

```zephir
var name = "peter";

echo "hello: " . name;
```

<a name='static-types'></a>

## Static Types

Статичні типи дозволяють розробнику оголосити та використовувати змінні з певними типами, які доступні у C. Змінна, яка оголошена з статичним типом не може змінювати свій тип. Проте, це дозволяє компілятору провести кращу оптимізацію. Підтримуються наступні типи даних:

| Тип                | Description                                                                    |
| ------------------ | ------------------------------------------------------------------------------ |
| `array`            | A structure that can be used as hash, map, dictionary, collection, stack, etc. |
| `boolean`          | A boolean expresses a truth value. It can be either `true` or `false`.         |
| `char`             | Smallest addressable unit of the machine that can contain basic character set. |
| `float`/`double`   | Double precision floating-point type. The size is platform-dependent.          |
| `integer`          | Signed integers. At least 16 bits in size.                                     |
| `long`             | Long signed integer type. At least 32 bits in size.                            |
| `string`           | A string is a series of characters, where a character is the same as a byte.   |
| `unsigned char`    | Same size as `char`, but guaranteed to be unsigned.                            |
| `unsigned integer` | Unsigned integers. Розмір не менше 16 біт.                                     |
| `unsigned long`    | Same as `long`, but unsigned.                                                  |

<a name='static-types-boolean'></a>

### Boolean

Логічний тип `boolean` виражає значення істини. It can be either `true` or `false`. На відміну від поведінки динамічного типу статичні логічні типи залишаються логічними (`true` or `false`), не залежно від того, яке значення їм призначається:

```zephir
boolean a;
let a = true;
```

##### automatically casted to `true`

```zephir
let a = 100;
```

##### automatically casted to `false`

```zephir
let a = 0;
```

##### throws a compiler exception

```zephir
let a = "hello";
```

<a name='static-types-char-unsigned'></a>

### Char/Unsigned Char

Змінна типу `char` найменша адресна одиниця машини, яка може містити символ з базового набору (як правило, 8 біт). Змінна типу `char` може використовуватися для зберігання будь-яких символів у рядку:

```zephir
char ch, string name = "peter";
```

##### stores 't'

```zephir
let ch = name[2];
```

##### `char` literals must be enclosed in single quotes

```zephir
let ch = 'Z';
```zephir

<a name='static-types-integer-unsigned'></a>
### Integer/Unsigned Integer
`integer` values are like the `integer` member in dynamic values. Values assigned to integer variables remain integer:

```zephir
int a;

let a = 50,
    a = -70;
```

##### automatically casted to 100

```zephir
let a = 100.25;
```

##### automatically casted to 0

```zephir
let a = null;
```

##### automatically casted to 0

```zephir
let a = false;
```

##### кидає виняток компіляції

```zephir
let a = "hello";
```

Беззнакові цілі `unsigned integer` схожі на цілі числа `integer`, але вони не мають знака, це означає, що ви не можете зберігати від’ємні числа в таких змінних:

```zephir
uint a;

let a = 50;
```

##### automatically casted to 70

```zephir
let a = -70;
```

##### автоматично перетворюється на 100

```zephir
let a = 100.25;
```

##### автоматично перетворюється на 0

```zephir
let a = null;
```

##### автоматично перетворюється на 0

```zephir
let a = false;
```

##### кидає виняток компіляції

```zephir
let a = "hello";
```

Тип `unsigned integer` вдвічі більший стандартного `integer`. Присвоєння беззнакових цілих стандартним цілим (знаковим) може привести до втрати даних:

##### potential loss of data for `b`

```zephir
uint a, int b;

let a = 2147483648,
    b = a;
```

<a name='static-types-long-unsigned'></a>

### Long/Unsigned Long

`long` variables are twice bigger than `integer` variables, thus they can store bigger numbers. As with `integer`, values assigned to `long` variables are automatically casted to this type:

```zephir
long a;

let a = 50,
    a = -70;
```

##### автоматично перетворюється на 100

```zephir
let a = 100.25;
```

##### автоматично перетворюється на 0

```zephir
    let a = null;
```

##### автоматично перетворюється на 0

```zephir
let a = false;
```

##### кидає виняток компіляції

```zephir
let a = "hello";
```

`unsigned long` are like `long` but they are not signed, this means you can't store negative numbers in these sort of variables:

```zephir
ulong a;

let a = 50;
```

##### автоматично перетворюється на 70

```zephir
let  a = -70;
```

##### автоматично перетворюється на 100

```zephir
let a = 100.25;
```

##### автоматично перетворюється на 0

```zephir
let a = null;
```

##### автоматично перетворюється на 0

```zephir
let a = false;
```

##### кидає виняток компіляції

```zephir
let a = "hello";
```

`unsigned long` variables are twice bigger than standard `long`; assigning `unsigned long` to standard (signed) `long` may result in loss of data:

##### можлива втрата даних для `b`

```zephir
ulong a, long b;

let a = 4294967296,
    b = a;
```

<a name='static-types-string'></a>

### String

A string is series of characters, where a character is the same as a byte. As in PHP it only supports a 256-character set, and hence does not offer native Unicode support.

Коли змінна оголошується як `string` вона ніколи не змінить свого типу:

```zephir
string a;

let a = "";
```

##### string literals must be enclosed in double quotes

```zephir
let  a = "hello";
```

##### converted to string "A"

```zephir
let a = 'A';
```

##### automatically casted to ""

```zephir
let a = null;
```