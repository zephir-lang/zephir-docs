* * *

layout: default language: 'ru-ru' version: '0.10'

* * *

# Управляющие структуры

Zephir реализует упрощенный набор управляющих структур, присутствующих в подобных ему языках, таких как C, PHP и т.п.

<a name='conditionals'></a>

## Условные

<a name='conditionals-if'></a>

### Оператор if

Условный оператор `if` реализует выполнение своего блока при условии, что некоторое логическое выражение (условие) принимает значение «истина» (`true`). Фигурные скобки обязательны. Оператор `if` может иметь необязательное предложение `else`. При необходимости, несколько конструкций `if`/`else` могут быть соединены вместе:

```zephir
if false {
    echo "ложь?";
} else {
    if true {
        echo "истина!";
    } else {
        echo "ни истина и ни ложь";
    }
}
```

Вы также можете использовать `elseif`:

```zephir
if a > 100 {
    echo "Значение a слишком большое";
} elseif a < 0 {
    echo "Значение a слишком маленькое";
} elseif a == 50 {
    echo "Превосходно!";
} else {
    echo "Хорошо";
}
```

Скобки в оцениваемом выражении необязательны:

```zephir
if a < 0 { return -1; } else { if a > 0 { return 1; } }
```

<a name='conditionals-switch'></a>

### Оператор switch

`switch` вычисляет выражение по ряду предопределенных литералов (буквальных значений), выполняя соответствующий блок `case` или в случае неудачи, выполняет блок `default`:

```zephir
switch count(items) {

    case 1:
    case 3:
        echo "odd items";
        break;

    case 2:
    case 4:
        echo "even items";
        break;

    default:
        echo "unknown items";
}
```

<a name='loops'></a>

## Циклы

<a name='loops-while'></a>

### Цикл while

`while` обозначает цикл, который повторяется до тех пор, пока его заданное условие оценивается как `true`:

```zephir
let counter = 5;
while counter {
    let counter -= 1;
}
```

<a name='loops-loop'></a>

### Цикл loop

В дополнение к `while `, `loop` может использоваться для создания бесконечных циклов:

```zephir
let n = 40;
loop {
    let n -= 2;
    if n % 5 == 0 { break; }
    echo x, "\n";
}
```

<a name='loops-for'></a>

### Цикл for

Цикл `for` является управляющей структурой, которая позволяет обходить массивы или строки:

```zephir
for item in ["a", "b", "c", "d"] {
    echo item, "\n";
}
```

Для доступа к ключам хешей, используется дополнительная переменная, как показано ниже:

```zephir
let items = ["a": 1, "b": 2, "c": 3, "d": 4];

for key, value in items {
    echo key, " ", value, "\n";
}
```

Цикл `for` также может быть использован для перемещения по массиву или строке в обратном порядке:

```zephir
let items = [1, 2, 3, 4, 5];

for value in reverse items {
    echo value, "\n";
}
```

Цикл `for` может использоваться для обхода строковых переменных:

```zephir
string language = "zephir"; char ch;

for ch in language {
    echo "[", ch ,"]";
}
```

В обратный порядке:

```zephir
string language = "zephir"; char ch;

for ch in reverse language {
    echo "[", ch ,"]";
}
```

Стандартный цикл `for` для обхода диапазона целых значений может быть записан следующим образом:

```zephir
for i in range(1, 10) {
    echo i, "\n";
}
```

Чтобы избежать предупреждений о неиспользуемых переменных, вы можете использовать анонимные переменные в операторах `for`, заменив имя переменной меткой `_`:

##### Использование ключа, но игнорирование значения

```zephir
for key, _ in data {
    echo key, "\n";
}
```

<a name='loops-break'></a>

### Оператор break

`break` завершает выполнение текущего оператора `while`, `for` или `loop` :

```zephir
for item in ["a", "b", "c", "d"] {
    if item == "c" {
        break; // exit the for
    }
    echo item, "\n";
}
```

<a name='loops-continue'></a>

### Оператор continue

`continue` используется внутри структур цикла для реализации пропуска оставшейся части итерации текущего цикла, продолжения выполнения после оценки состояния и перехода к началу следующей итерации.

```zephir
let a = 5;
while a > 0 {
    let a--;
    if a == 3 {
        continue;
    }
    echo a, "\n";
}
```

<a name='require'></a>

## Оператор require

The `require` statement dynamically includes and evaluates a specified PHP file. Note that files included via Zephir are interpreted by Zend Engine as normal PHP files. `require` does not allow Zephir code to include other Zephir files at runtime.

```zephir
if file_exists(path) {
    require path;
}
```

<a name='let'></a>

## Оператор let

The `let` statement is used to mutate variables, properties and arrays. Variables are by default immutable and this instruction makes them mutable for the duration of the statement:

```zephir
let name = "Tony";           // simple variable
let this->name = "Tony";     // object property
let data["name"] = "Tony";   // array index
let self::_name = "Tony";    // static property
```

Also this instruction must be used to increment/decrement variables:

```zephir
let number++;           // increment simple variable
let number--;           // decrement simple variable
let this->number++;     // increment object property
let this->number--;     // decrement object property
```

Multiple mutations can be performed in a single `let` operation:

```zephir
let price = 1.00, realPrice = price, status = false;
```