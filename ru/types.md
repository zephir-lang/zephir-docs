# Типы

Zephir динамически и статически типизированный. В этой главе мы выделим поддерживаемые типы и их поведение.

<a name='dynamic-types'></a>

## Динамические типы

Динамические переменные точно такие же, как и в PHP. Они могут быть назначены и переназначены в различные типы без ограничений.

Динамическая переменная должна быть объявлена с помощью служебного слова `var`. Работа с такой переменной очень похожа на то, как мы работаем с переменными в PHP:

    var a, b, c;
    

##### Инициализировать переменные

    let a = "hello", b = false;
    

##### Изменить их значения

    let a = "hello", b = false;
    let a = 10, b = "140";
    

##### Выполнить операции с ними

    let c = a + b;
    

Переменные объявленные как динамические, могут иметь восемь типов:

| Тип              | Описание                                                                                 |
| ---------------- | ---------------------------------------------------------------------------------------- |
| `array`          | Массив — это упорядоченная карта. Карта — это тип, который связывает значения с ключами. |
| `boolean`        | Булев тип выражает истинное значение. Он может иметь значения `true` либо `false`        |
| `float`/`double` | Числа с плавающей точкой. Размер зависит от платформы.                                   |
| `integer`        | Целые числа. Размер целого числа зависит от платформы.                                   |
| `null`           | Специальное значение NULL представляет переменную без значения.                          |
| `object`         | Абстракция объекта, как и в PHP.                                                         |
| `resource`       | Ресурс содержит ссылку на внешний ресурс.                                                |
| `string`         | Строка представляет собой последовательность символов, где символ совпадает с байтом.    |

Подробнее об этих типах можно узнать в [руководстве PHP](http://www.php.net/manual/en/language.types.php).

<a name='dynamic-types-arrays'></a>

### Array

Реализация массива в Zephir в основном такая же, как и в PHP: упорядоченные карты, оптимизированные для различных целей; Этот тип данных можно трактовать как массив, список (вектор), хэш-таблицу (реализацию карты), словарь, коллекцию, стек, очередь. Возможны и другие трактовки. Много мерные массивы также возможны: в качестве значений массива могут выступать другие массивы.

Синтаксис для определения массивов немного отличается от PHP:

##### Для определения массивов должны использоваться квадратные скобки

    let myArray = [1, 2, 3];
    

##### Для определения ключей хэшей необходимо использовать двоеточие

    let myHash = ["раз": 1, "два": 2, "три": 3];
    

В качестве ключей могут использоваться только значения типов long и string:

    let myHash = [0: "раз", 1: true, 2: null];
    let myHash = ["раз": 7.0, "два": "некоторая строка", "три": false];
    

<a name='dynamic-types-boolean'></a>

### Boolean

Булев тип выражает истинное значение. Он может иметь значения `true` либо `false`:

    var a = false, b = true;
    

<a name='dynamic-types-float-double'></a>

### Float/Double

Числа с плавающей точкой (также известные как "вещественные числа"). Литералы с плавающей точкой представляют собой выражения из нуля или более цифр, за которыми следует точка (.), за которой следуют ноль или более цифр. Размер float — платформозависимая величина, хотя максимум ~ 1.8e308 с точностью примерно 14 десятичных цифр является наиболее общим значением (64-битный формат IEEE).

    var number = 5.0, b = 0.014;
    

Числа с плавающей точкой имеют ограниченную точность. Хотя это зависит от системы, как и в PHP, Zephir использует формат двойной точности IEEE 754, который даст максимальную относительную ошибку из-за округления порядка 1.11e-16.

<a name='dynamic-types-integer'></a>

### Integer

Целые числа. Размер целого числа зависит от платформы, хотя максимальное значение около двух миллиардов является обычным значением (это 32 битное, знаковое). 64-битные платформы обычно имеют максимальное значение около 9E18. PHP не поддерживает беззнаковые целые, поэтому у Zephir есть это ограничение тоже:

    var a = 5, b = 10050;
    

<a name='dynamic-types-integer-overflow'></a>

### Целочисленное переполнение

В отличие от PHP, Zephir автоматически не проверяет переполнение целочисленного типа. Точно так же, как и в C, если вы выполняете операции, которые могут возвращать большое число, вы можете использовать такие типы, как `unsigned long` или `float` для их хранения:

    unsigned long my_number = 2147483648;
    

<a name='dynamic-types-objects'></a>

### Object

Zephir позволяет создавать экземпляры PHP классов, манипулировать PHP объектами, вызывать методы на них, читать константы классов и т.д.:

    let myObject = new \stdClass(),
        myObject->someProperty = "некоторое значение";
    

<a name='dynamic-types-string'></a>

### String

Строка представляет собой последовательность символов, где символ (char) является одним байтом. Как PHP, Zephir поддерживает только 256-символьный набор и, следовательно, не предлагает поддержку Unicode.

    var today = "friday";
    

В Zephir строковые литералы могут указываться только с помощью двойных кавычек (как в C или Go). Одинарные кавычки зарезервированы для типа данных char.

В строках поддерживаются следующие escape-последовательности:

| Последовательность | Описание                 |
| ------------------ | ------------------------ |
| `\\t`            | Горизонтальная табуляция |
| `\\n`            | Перевод строки           |
| `\\r`            | Возврат каретки          |
| `\\ \`        | Обратная косая черта     |
| `\\"`            | Двойная кавычка          |

    var today    = "\tпятница\n\r",
        tomorrow = "\tсуббота";
    

В Zephir строки не поддерживают интерполяцию переменных, как в PHP, вместо этого вы можете использовать конкатенацию:

    var name = "peter";
    
    echo "hello: " . name;
    

<a name='static-types'></a>

## Статические типы

Статическая типизация позволяет разработчику объявлять и использовать некоторые типы переменных, доступные в C. Переменные, объявленные как статические, не могут менять свой тип. Тем не менее, они позволяют компилятору делать оптимизацию лучше. Поддерживаются следующие типы данных:

| Тип                | Описание                                                                                          |
| ------------------ | ------------------------------------------------------------------------------------------------- |
| `array`            | Структура, которая может использоваться в качестве хеша, карты, словаря, коллекции, стека и т. д. |
| `boolean`          | Булев тип выражает истинное значение. Он может иметь значения `true` либо `false`.                |
| `char`             | Наименьшая адресуемая единица машины, которая может содержать символ из базового набора.          |
| `float`/`double`   | Тип с плавающей точкой двойной точности. Размер является платформозависимым.                      |
| `integer`          | Знаковое целое. Размер по крайней мере 16 бит.                                                    |
| `long`             | Длинное знаковое целое. Размер по крайней мере 32 бит.                                            |
| `string`           | Строка представляет собой последовательность символов, где символ (char) является одним байтом.   |
| `unsigned char`    | Тот же размер, что и char, но гарантированно беззнаковый.                                         |
| `unsigned integer` | Беззнаковое целое. Размер по крайней мере 16 бит.                                                 |
| `unsigned long`    | То же размер, что и long, но беззнаковый.                                                         |

<a name='static-types-boolean'></a>

### Boolean

Булев тип выражает истинное значение. Он может иметь значения `true` либо `false`. В отличие от динамического поведения статические логические типы остаются логическими (true или false), а не тем типом значения, что им присваивается:

    boolean a;
    let a = true;
    

##### автоматически переводится в `true`

    let a = 100;
    

##### автоматически переводится в `false`

    let a = 0
    

##### выкидывает ошибку компиляции

    let a = "hello";
    

<a name='static-types-char-unsigned'></a>

### Char/Unsigned Char

Переменная типа сhar — наименьшая адресуемая единица машины, которая может содержать символ из базового набора (обычно 8 бит). Переменная типа `char` может быть использована для хранения одного символа в строке:

    char ch, string name = "peter";
    

##### сохраняет 't'

    let ch = name[2];
    

##### литералы типа `char` должны быть заключены в одинарные кавычки

    let ch = 'Z';
    

<a name='static-types-integer-unsigned'></a>

### Integer/Unsigned Integer

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
    

##### выкидывает ошибку компиляции

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
    

##### выкидывает ошибку компиляции

    let a = "hello";
    

Unsigned integers are twice bigger than standard integers. Assigning unsigned integers to standard (signed) integers may result in loss of data:

##### potential loss of data for `b`

    uint a, int b;
    
    let a = 2147483648,
        b = a;
    

<a name='static-types-long-unsigned'></a>

### Long/Unsigned Long

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
    

##### выкидывает ошибку компиляции

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
    

##### выкидывает ошибку компиляции

    let a = "hello";
    

Unsigned longs are twice bigger than standard longs; assigning unsigned longs to standard (signed) longs may result in loss of data:

##### potential loss of data for `b`

    ulong a, long b;
    
    let a = 4294967296,
        b = a;
    

<a name='static-types-string'></a>

### String

A string is series of characters, where a character is the same as a byte. As in PHP it only supports a 256-character set, and hence does not offer native Unicode support.

When a variable is declared `string` it never changes its type:

    string a;
    
    let a = "";
    

##### string literals must be enclosed in double quotes

    let  a = "hello";
    

##### converted to string "A"

    let a = 'A';
    

##### automatically casted to ""

    let a = null;