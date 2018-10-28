# Типи

Zephir поєднує в собі статичну та динамічну типізацію. У цьому розділі ми підкреслимо підтримувані типи та їхню поведінку.

<a name='dynamic-types'></a>

## Динамічні типи

Динамічних змінні працюють так само, як і в PHP. Їм можна призначати та перепризначати значення різних типів без обмежень.

Динамічна змінна має бути оголошеною з ключовим словом `var`. Поведінка майже така сама, як і в PHP:

    var a, b, c;
    

##### Ініціалізація змінних

    let a = "hello", b = false;
    

##### Зміна їхнього значення

    let a = "hello", b = false;
    let a = 10, b = "140";
    

##### Виконання операцій

    let c = a + b;
    

Вони можуть бути восьми типів:

| Тип              | Опис                                                                                                  |
| ---------------- | ----------------------------------------------------------------------------------------------------- |
| `array`          | Масив — це впорядкована мапа. Мапа — це тип, який встановлює відповідність між значеннями та ключами. |
| `boolean`        | Булевий тип виражає значення істини. Він може бути `true` або `false`.                                |
| `float`/`double` | Число з рухомою комою. Розмір числа залежить від платформи.                                           |
| `integer`        | Цілі числа. Розмір числа залежить від платформи.                                                      |
| `null`           | Особливе значення NULL, яке означає змінну в якої немає значення.                                     |
| `object`         | Абстракція об'єкта як у PHP.                                                                          |
| `resource`       | Ресурс містить посилання на зовнішній ресурс.                                                         |
| `string`         | Рядок є послідовністю символів, де кожен символ є одним байтом.                                       |

Більше про типи ви можете дізнатися в [Документації PHP](http://www.php.net/manual/en/language.types.php).

<a name='dynamic-types-arrays'></a>

### Array

Реалізація масивів у Zephir в основному така сама як у PHP: впорядковані мапи оптимізовані для деяких випадків; можна розглядати як масив список (вектор) хеш-таблицю (реалізація мапи), словник, колекція, стек, черги. Можливі й інші трактування. Значеннями масиву можуть бути інші масиви, дерева, та багатовимірні масиви.

Синтаксис оголошення масиву дещо відрізняється від PHP:

##### Для оголошення повинні використовуватися квадратні дужки

    let myArray = [1, 2, 3];
    

##### Для оголошення масиву з ключами повинна використовуватися двокрапка

    let myHash = ["first": 1, "second": 2, "third": 3];
    

Ключами масиву можуть лише цілі числа та рядки:

    let myHash = [0: "first", 1: true, 2: null];
    let myHash = ["first": 7.0, "second": "some string", "third": false];
    

<a name='dynamic-types-boolean'></a>

### Boolean

Булевий тип виражає значення істини. Він може бути `true` або `false`:

    var a = false, b = true;
    

<a name='dynamic-types-float-double'></a>

### Float/Double

Числа з рухомою комою (також відомі як "двійкові", "числа з подвійною точністю", "дійсні числа"). Літерали з рухомою комою - це вирази з однією або кількома цифрами, а потім - крапка (.), після нього одна або кілька цифр. Розмір числа після крапки залежить від платформи, хоча максимум ~1.8e308 з точністю приблизно 14 десяткових цифр мають загальне значення (64-бітний форматі IEEE).

    var number = 5.0, b = 0.014;
    

Числа з рухомою комою мають обмежену точність. Хоча це залежить від системи, як і в PHP, Zephir використовує формат подвійної точності IEEE 754, який дасть максимальну відносну помилку через округлення порядку 1.11e-16.

<a name='dynamic-types-integer'></a>

### Integer

Цілі числа. Розмір числа залежить від платформи, хоча максимальне значення для 32-бітного знакового числа є 2,147,483,647. 64-розрядні платформи зазвичай мають максимальне значення близько 9E18. PHP не підтримує цілі числа без знаку, тому Zephir теж має це обмеження:

    var a = 5, b = 10050;
    

<a name='dynamic-types-integer-overflow'></a>

### Програмне переповнення цілих чисел

На відміну від PHP Zephir автоматично не перевіряє рівень заповненості цілого числа. Як і в C, якщо ви виконуєте операції які можуть повернути велике число ви повинні використати такі типи як `unsigned long` або ж `float`:

    unsigned long my_number = 2147483648;
    

<a name='dynamic-types-objects'></a>

### Object

Zephir дозволяє створювати екземпляри PHP класів, маніпулювати PHP-об'єктами, викликати методи, читати константи класу та інші речі, які дозволяють PHP-об'єкти:

    let myObject = new \stdClass(),
        myObject->someProperty = "my value";
    

<a name='dynamic-types-string'></a>

### String

A `string` is series of characters, where a character is the same as a byte. Як і PHP, Zephir підтримує лише 256-символьний набір, а отже не дає вбудованої підтримки Unicode.

    var today = "friday";
    

У Zephir рядкові літерали можна задавати лише взявши їх у подвійні лапки (як у C або Go). Single quotes are reserved for `char` data type.

У рядках підтримуються наступні символи екранування:

| Послідовність | Опис                   |
| ------------- | ---------------------- |
| `\t`         | Горизонтальний відступ |
| `\n`         | Переведення рядка      |
| `\r`         | Повернення каретки     |
| `\`        | Бекслеш                |
| `\"`         | Подвійні лапки         |

    var today    = "\tfriday\n\r",
        tomorrow = "\tsaturday";
    

Zephir не підтримує інтерполяцію змінних як це було в PHP; натомість ви повинні використовувати конкатенацію:

    var name = "peter";
    
    echo "hello: " . name;
    

<a name='static-types'></a>

## Статичні типи

Статичні типи дозволяють розробнику оголосити та використовувати змінні з певними типами, які доступні у C. Змінна, яка оголошена з статичним типом не може змінювати свій тип. Проте, це дозволяє компілятору провести кращу оптимізацію. Підтримуються наступні типи даних:

| Тип                | Опис                                                                                |
| ------------------ | ----------------------------------------------------------------------------------- |
| `array`            | Структура, яка може бути використана як хеш, мапа, словник, колекція, стек, і т. д. |
| `boolean`          | Булевий тип виражає значення істини. Він може бути `true` або `false`.              |
| `char`             | Найменша адресна одиниця машини, яка може містити символ з базового набору.         |
| `float`/`double`   | Тип з рухомою комою подвійної точності. Розмір числа залежить від платформи.        |
| `integer`          | Знакові числа. Розмір не менше 16 біт.                                              |
| `long`             | Довге знакове число. Розмір не менше 32 біт.                                        |
| `string`           | Рядок є послідовністю символів, де кожен символ є одним байтом.                     |
| `unsigned char`    | Same size as `char`, but guaranteed to be unsigned.                                 |
| `unsigned integer` | Беззнакове ціле. Розмір не менше 16 біт.                                            |
| `unsigned long`    | Same as `long`, but unsigned.                                                       |

<a name='static-types-boolean'></a>

### Boolean

A `boolean` expresses a truth value. Він може бути `true` або `false`. Contrary to the dynamic behavior detailed above, static `boolean` types remain `boolean` (`true` or `false`) no mater what value is assigned to them:

    boolean a;
    let a = true;
    

##### автоматично перетворюється на `true`

    let a = 100;
    

##### автоматично перетворюється на `false`

    let a = 0;
    

##### кидає виняток компіляції

    let a = "hello";
    

<a name='static-types-char-unsigned'></a>

### Char/Unsigned Char

`char` variables are the smallest addressable unit of the machine that can contain the basic character set (generally 8 bits). Змінна типу `char` може використовуватися для зберігання будь-яких символів у рядку:

    char ch, string name = "peter";
    

##### stores 't'

    let ch = name[2];
    

##### `char` literals must be enclosed in single quotes

    let ch = 'Z';
    

<a name='static-types-integer-unsigned'></a>

### Integer/Unsigned Integer

`integer` values are like the `integer` member in dynamic values. Values assigned to integer variables remain integer:

    int a;
    
    let a = 50,
        a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### кидає виняток компіляції

    let a = "hello";
    

`unsigned integer` variables are like `integer` but they don't have sign, this means you can't store negative numbers in these sort of variables:

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
    

##### кидає виняток компіляції

    let a = "hello";
    

`unsigned integer` variables are twice bigger than standard `integer`. Assigning `unsigned integer` to standard (signed) `integer` may result in loss of data:

##### potential loss of data for `b`

    uint a, int b;
    
    let a = 2147483648,
        b = a;
    

<a name='static-types-long-unsigned'></a>

### Long/Unsigned Long

`long` variables are twice bigger than `integer` variables, thus they can store bigger numbers. As with `integer`, values assigned to `long` variables are automatically casted to this type:

    long a;
    
    let a = 50,
        a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### кидає виняток компіляції

    let a = "hello";
    

`unsigned long` are like `long` but they are not signed, this means you can't store negative numbers in these sort of variables:

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
    

##### кидає виняток компіляції

    let a = "hello";
    

`unsigned long` variables are twice bigger than standard `long`; assigning `unsigned long` to standard (signed) `long` may result in loss of data:

##### potential loss of data for `b`

    ulong a, long b;
    
    let a = 4294967296,
        b = a;
    

<a name='static-types-string'></a>

### String

Рядок є послідовністю символів, де кожен символ є одним байтом. As in PHP it only supports a 256-character set, and hence does not offer native Unicode support.

Коли змінна оголошується як `string` вона ніколи не змінить свого типу:

    string a;
    
    let a = "";
    

##### string literals must be enclosed in double quotes

    let  a = "hello";
    

##### converted to string "A"

    let a = 'A';
    

##### automatically casted to ""

    let a = null;