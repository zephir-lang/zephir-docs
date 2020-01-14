---
layout: default
language: 'uk-ua'
version: '0.11'
---

# Оператори
Оператори в Zephir схожі на оператори в PHP й поводяться так само.

<a name='arithmetic-operators'></a>

## Арифметичні оператори
Підтримуються наступні оператори:

| Операція                  | Приклад |
| ------------------------- | ------- |
| Заперечення (зміна знаку) | `-a`    |
| Додавання                 | `a + b` |
| Віднімання                | `a - b` |
| Множення                  | `a * b` |
| Ділення                   | `a / b` |
| Ділення по модулю         | `a % b` |

<a name='bitwise-operators'></a>

## Бітові оператори
Підтримуються наступні оператори:

| Операція                             | Приклад        |
| ------------------------------------ | -------------- |
| І (кон'юнкція)                       | `a & b`    |
| Або (диз'юнкція)                     | `a | b`        |
| Виключення або (виключна диз'юнкція) | `a ^ b`        |
| Заперечення                          | `~a`           |
| Зсув ліворуч                         | `a << b` |
| Зсув праворуч                        | `a >> b` |

Приклад:

```zephir
if a & SOME_FLAG {
    echo "Прапорець встановлений";
}
```

Дізнатися більше про порівняння динамічних змінних можна з [документації по PHP](http://www.php.net/manual/en/language.operators.comparison.php).

<a name='comparison-operators'></a>

## Оператори порівняння
Операції порівняння залежать від типу порівнюваних змінних. Наприклад, якщо обидва операнди динамічні, то результат буде таким же як і в PHP:

| Приклад        | Операція                  | Опис                                                        |
| -------------- | ------------------------- | ----------------------------------------------------------- |
| `a == b`       | Рівність                  | `true`, якщо a дорівнює b після приведення типів.           |
| `a === b`      | Ідентичність (тотожність) | `true`, якщо a дорівнює b, і операнди одного типу.          |
| `a != b`       | Нерівність                | `true`, якщо a не дорівнює b після приведення типів.        |
| `a <> b` | Нерівність                | `true`, якщо a не дорівнює b після приведення типів.        |
| `a !== b`      | Не ідентичність (різниця) | `true`, якщо a не дорівнює b або коли в операнд різні типи. |
| `a < b`     | Менше ніж                 | `true`, якщо a строго менше b.                              |
| `a > b`     | Більше ніж                | `true`, якщо a строго більше b.                             |
| `a <= b`    | Менше ніж або дорівнює    | `true`, якщо a менше або дорівнює b.                        |
| `a >= b`    | Більше ніж або дорівнює   | `true`, якщо a більше або дорівнює b.                       |

Приклад:

```zephir
if a == b {
    return 0;
} else {
    if a < b {
        return -1;
    } else {
        return 1;
    }
}
```

<a name='logical-operators'></a>

## Логічні оператори
Підтримуються наступні оператори:

| Операція       | Приклад          |
| -------------- | ---------------- |
| І (кон'юнкція) | `a && b` |
| Або            | `a || b`         |
| Заперечення    | `!a`             |

Приклад:

```zephir
if a && b || !c {
    return -1;
}
return 1;
```

<a name='tenary-operator'></a>

## Тернарний оператор
Zephir підтримує тернарний оператор, як в C або PHP:

```zephir
let b = a == 1 ? "x" : "y"; // b буде присвоєно "x", якщо a дорівнює 1, інакше "y"
```

<a name='special-operators'></a>

## Спеціальні оператори
Підтримуються наступні оператори:

<a name='special-operators-empty'></a>

### Empty
Цей оператор дозволяє перевірити вираз на порожнечу. Під "порожнечею" мається на увазі вираз, який дорівнює `null`, порожньому рядку або порожньому масиву:

```zephir
let someVar = "";
if empty someVar {
    echo "порожньо!";
}

let someVar = "hello";
if !empty someVar {
    echo "не порожньо!";
}
```

<a name='special-operators-fetch'></a>

### Fetch
Оператор "fetch" створений для скорочення популярної в PHP конструкції:

```php
<?php

if (isset($myArray[$key])) {
    $value = $myArray[$key];
    echo $value;
}
```

У Zephir ви можете написати той самий код так:

```zephir
if fetch value, myArray[key] {
    echo value;
}
```

Оператор "fetch" поверне `true`, якщо в масиві присутній ключ `key` і присвоїть значення змінній `value`.

<a name='special-operators-isset'></a>

### Isset
Перевіряє, чи існує індекс у масиву або властивість у об'єкта:

```zephir
let someArray = ["a": 1, "b": 2, "c": 3];
if isset someArray["b"] { // перевіряє чи є в масиву індекс "b"
    echo "yes, it has an index 'b'\n";
}
```

Використання `isset` можливе і в конструкціях повернення:

```zephir
return isset this->{someProperty};
```

Зверніть увагу, що `isset` у Zephir працює швидше як функція [array_key_exists](http://www.php.net/manual/en/function.array-key-exists.php) в PHP. Тобто `isset` поверне `true` навіть, якщо значення масиву або властивість об'єкту дорівнює `null`.

<a name='special-operators-typeof'></a>

### Typeof
Цей оператор перевіряє тип змінної. Оператор `typeof` можна використовувати з порівняльним оператором:

```zephir
if (typeof str == "string") { // or !=
    echo str;
}
```

Він також може працювати як PHP-функція `gettype`.

```zephir
return typeof str;
```

**Be careful**, if you want to check whether an object is 'callable', you always have to use `typeof` as a comparison operator, not a function.

<a name='special-operators-type-hints'></a>

### Підказка типа
Zephir завжди намагається перевірити, чи реалізує об’єкт методи та властивості викликання/доступу до змінної, яка виводиться як об’єкт:

```zephir
let o = new MyObject();

// Zephir перевіряє чи реалізований метод "myMethod" в MyObject
o->myMethod();
```

Проте через динамічну природу, яка успадкована від PHP, іноді нелегко дізнатися клас об'єкта, тому Zephir не може ефективно створювати звіти про помилки. Підказка типу повідомляє компілятору, який клас пов'язаний з динамічною змінною, що дозволяє компілятору виконувати більше перевірок компіляції:

```zephir
// Повідомляє компілятору, що "o"
// є екземпляром класу MyClass
let o = <MyClass> this->_myObject;
o->myMethod();
```

These "type hints" are weak. This means the program does not check if the value is in fact an instance of the specified class, nor whether it implements the specified interface. If you want it to check this every time in execution, use a strict type:

```zephir
// Always check if the property is an instance
// of MyClass before the assignment
let o = <MyClass!> this->_myObject;
o->myMethod();
```

<a name='special-operators-branch-prediction-hints'></a>

### Branch Prediction Hints
What is branch prediction? Check this [article](http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/) or refer to the [Wikipedia article](https://en.wikipedia.org/wiki/Branch_predictor). In environments where performance is very important, it may be useful to introduce these hints.

Consider the following example:

```zephir
let allPaths = [];
for path in this->_paths {
    if path->isAllowed() == false {
        throw new App\Exception("Some error message here");
    } else {
        let allPaths[] = path;
    }
}
```

The authors of the above code know in advance that the condition that throws the exception is unlikely to happen. This means that, 99.9% of the time, our method executes that condition, but it is probably never evaluated as true. For the processor, this could be hard to know, so we could introduce a hint there:

```zephir
let allPaths = [];
for path in this->_paths {
    if unlikely path->isAllowed() == false {
        throw new App\Exception("Some error message here");
    } else {
        let allPaths[] = path;
    }
}
```
