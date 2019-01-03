---
layout: default
language: 'ru-RU'
version: '0.11'
menu:
  - text:
      'Арифметические операторы'
    url: '#arithmetic-operators'
  - text:
      'Побитовые операторы'
    url: '#bitwise-operators'
  - text:
      'Операторы сравнения'
    url: '#comparison-operators'
  - text:
      'Логические операторы'
    url: '#logical-operators'
  - text:
      'Тернарный оператор'
    url: '#tenary-operator'
  - text:
      'Специальные операторы'
    url: '#special-operators'
    sub:
      - text:
            'Empty'
        url: '#special-operators-empty'
      - text:
            'Fetch'
        url: '#special-operators-fetch'
      - text:
            'Isset'
        url: '#special-operators-isset'
      - text:
            'Подсказки типа'
        url: '#special-operators-type-hints'
      - text:
            'Подсказки прогнозирования ветвлений'
        url: '#special-operators-branch-prediction-hints'
---
# Операторы

Операторы в Zephir похожи на их аналоги в PHP и также себя ведут.

<a name='arithmetic-operators'></a>

## Арифметические операторы

Поддерживаются следующие операторы:

| Операция                    | Пример  |
| --------------------------- | ------- |
| Приведение к отрицательному | `-a`    |
| Сложение                    | `a + b` |
| Вычитание                   | `a - b` |
| Умножение                   | `a * b` |
| Деление                     | `a / b` |
| Деление по модулю           | `a % b` |

<a name='bitwise-operators'></a>

## Побитовые операторы

Поддерживаются следующие операторы:

| Операция        | Пример         |
| --------------- | -------------- |
| И               | `a & b`    |
| Или             | `a | b`        |
| Исключающее или | `a ^ b`        |
| Отрицание       | `~a`           |
| Сдвиг влево     | `a << b` |
| Сдвиг вправо    | `a >> b` |

Пример:

    if a & SOME_FLAG {
        echo "Флаг установлен";
    }
    

Узнать больше о сравнении динамических переменных можно из [документации по PHP](http://www.php.net/manual/en/language.operators.comparison.php).

<a name='comparison-operators'></a>

## Операторы сравнения

Операции сравнения зависят от типа сравниваемых переменных. Например если оба операнда динамические, то результат будет таким же как и в PHP:

| Пример         | Операция             | Description                                      |
| -------------- | -------------------- | ------------------------------------------------ |
| `a == b`       | Равенство            | `true` если a равно b после приведения типов.    |
| `a === b`      | Идентичность         | `true` если a равно b, и операнды одного типа.   |
| `a != b`       | Не равны             | `true` если a не равно b после приведения типов. |
| `a <> b` | Не равны             | `true` если a не равно b после приведения типов. |
| `a !== b`      | Не идентичны         | `true` если a не равно b после приведения типов. |
| `a < b`     | Меньше чем           | `true` если a строго меньше b.                   |
| `a > b`     | Больше чем           | `true` если a строго больше b.                   |
| `a <= b`    | Меньше чем или равно | `true` если a меньше, или равно b.               |
| `a >= b`    | Больше чем или равно | `true` если a строго больше, или равно b.        |

Пример:

    if a == b {
        return 0;
    } else {
        if a < b {
            return -1;
        } else {
            return 1;
        }
    }
    

<a name='logical-operators'></a>

## Логические операторы

Поддерживаются следующие операторы:

| Операция  | Пример           |
| --------- | ---------------- |
| И         | `a && b` |
| Или       | `a || b`         |
| Отрицание | `!a`             |

Пример:

    if a && b || !c {
        return -1;
    }
    return 1;
    

<a name='tenary-operator'></a>

## Тернарный оператор

Zephir поддерживает тернарный оператор, как в C или PHP:

    let b = a == 1 ? "x" : "y"; // b будет присвоен "x", если a равно 1 в противном случае "y"
    

<a name='special-operators'></a>

## Специальные операторы

Поддерживаются следующие операторы:

<a name='special-operators-empty'></a>

### Empty

Этот оператор позволяет узнать пусто ли выражение. Под "пусто" подразумевается выражение равное `null`, пустая строка или пустой массив:

    let someVar = "";
    if empty someVar {
        echo "пусто!";
    }
    
    let someVar = "привет";
    if !empty someVar {
        echo "не пусто!";
    }
    

<a name='special-operators-fetch'></a>

### Fetch

Оператор "fetch" создан для сокращения популярной в PHP конструкции:

    <?php
    
    if (isset($myArray[$key])) {
        $value = $myArray[$key];
        echo $value;
    }
    

В Zephir тот же код будет можно написать так:

    if fetch value, myArray[key] {
        echo value;
    }
    

Оператор "fetch" вернет `true`, если в массиве есть что-то по ключу "key" и тогда переменной "value" будет присвоено значение.

<a name='special-operators-isset'></a>

### Isset

Проверяет, существует ли индекс у массива или свойство у объекта:

    let someArray = ["a": 1, "b": 2, "c": 3];
    if isset someArray["b"] { // проверим, есть ли у массива индекс "b"
        echo "Да, переменная имеет индекс 'b'\n";
    }
    

Использование `isset` возможно и в возвращаемых конструкциях:

    return isset this->{someProperty};
    

Обратите внимание, `isset` в Zephir работает скорее как [array_key_exists](http://www.php.net/manual/en/function.array-key-exists.php) в PHP. Иными словами `isset` в Zephir вернёт true, даже если значение равно null.

<a name='special-operators-typeof'></a>

### Typeof

This operator checks a variable's type. 'typeof' can be used with a comparison operator:

    if (typeof str == "string") { // or !=
        echo str;
    }
    

It can also work like the PHP function `gettype`.

    return typeof str;
    

**Be careful**, if you want to check whether an object is 'callable', you always have to use `typeof` as a comparison operator, not a function.

<a name='special-operators-type-hints'></a>

### Подсказки типа

Zephir всегда пытается проверить, реализует ли объект методы и свойства, вызываемые/доступные для переменной, которая выводится как объект:

    let o = new MyObject();
    
    // Zephir проверяет, реализован ли "myMethod" в MyObject
    o->myMethod();
    

Однако из-за динамичной природы, унаследованной от PHP, иногда нелегко узнать класс объекта, поэтому Zephir не может эффективно создавать отчеты об ошибках. Подсказка типа сообщает компилятору, какой класс связан с динамической переменной, позволяющей компилятору выполнять больше проверок компиляции:

    // Сообщает компилятору, что "o"
    // является экземпляром класса MyClass
    let o = <MyClass> this->_myObject;
    o->myMethod();
    

These "type hints" are weak. This means the program does not check if the value is in fact an instance of the specified class, nor whether it implements the specified interface. If you want it to check this every time in execution, use a strict type:

    // Always check if the property is an instance
    // of MyClass before the assignment
    let o = <MyClass!> this->_myObject;
    o->myMethod();
    

<a name='special-operators-branch-prediction-hints'></a>

### Подсказки прогнозирования ветвлений

Что такое прогнозирование ветвлений? Для подробного описания это понятия обратитесь к [статье Игоря Островского](http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/) или [описанию на Wikipedia](https://en.wikipedia.org/wiki/Branch_predictor). В окружениях, где производительность является очень важной составляющей, может оказаться полезным использование подсказок при прогнозировании ветвлений.

Рассмотрим следующий пример:

    let allPaths = [];
    for path in this->_paths {
        if path->isAllowed() == false {
            throw new App\Exception("Некое сообщение об ошибке");
        } else {
            let allPaths[] = path;
        }
    }
    

Авторы кода, приведенного выше, заранее знают, что условие, которое выбрасывает исключение, вряд ли произойдет. Это означает, что в 99.9% случаев наш метод выполняет эту проверку в холостую — условие вероятно не будет оцениваться как истинное. Но для процессора, обычно, это сложно понять, поэтому мы могли бы ввести здесь подсказку:

    let allPaths = [];
    for path in this->_paths {
        if unlikely path->isAllowed() == false {
            throw new App\Exception("Некое сообщение об ошибке");
        } else {
            let allPaths[] = path;
        }
    }