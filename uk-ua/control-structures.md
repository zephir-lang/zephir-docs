* * *

layout: default language: 'uk' version: '0.10'

* * *

# Control Structures

Zephir implements a simplified set of control structures present in similar languages like C, PHP etc.

<a name='conditionals'></a>

## Conditionals

<a name='conditionals-if'></a>

### If Statement

`if` statements evaluate an expression, executing the following block if the evaluation is `true`. Braces are required. An `if` can have an optional `else` clause, and multiple `if`/`else` constructs can be chained together:

```zephir
if false {
    echo "false?";
} else {
    if true {
        echo "true!";
    } else {
        echo "neither true nor false";
    }
}
```

`elseif` clauses are also available:

```zephir
if a > 100 {
    echo "to big";
} elseif a < 0 {
    echo "to small";
} elseif a == 50 {
    echo "perfect!";
} else {
    echo "ok";
}
```

Parentheses in the evaluated expression are optional:

```zephir
if a < 0 { return -1; } else { if a > 0 { return 1; } }
```

<a name='conditionals-switch'></a>

### Switch Statement

A `switch` evaluates an expression against a series of predefined literal values, executing the corresponding `case` block or falling back to the `default` block case:

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

## Loops

<a name='loops-while'></a>

### While Statement

`while` denotes a loop that iterates as long as its given condition evaluates as `true`:

```zephir
let counter = 5;
while counter {
    let counter -= 1;
}
```

<a name='loops-loop'></a>

### Loop Statement

In addition to `while`, `loop` can be used to create infinite loops:

```zephir
let n = 40;
loop {
    let n -= 2;
    if n % 5 == 0 { break; }
    echo x, "\n";
}
```

<a name='loops-for'></a>

### For Statement

A `for` is a control structure that allows to traverse arrays or strings:

```zephir
for item in ["a", "b", "c", "d"] {
    echo item, "\n";
}
```

Keys in hashes can be obtained by providing a variable for both the key and value:

```zephir
let items = ["a": 1, "b": 2, "c": 3, "d": 4];

for key, value in items {
    echo key, " ", value, "\n";
}
```

A `for` loop can also be instructed to traverse an array or string in reverse order:

```zephir
let items = [1, 2, 3, 4, 5];

for value in reverse items {
    echo value, "\n";
}
```

A `for` loop can be used to traverse string variables:

```zephir
string language = "zephir"; char ch;

for ch in language {
    echo "[", ch ,"]";
}
```

In reverse order:

```zephir
string language = "zephir"; char ch;

for ch in reverse language {
    echo "[", ch ,"]";
}
```

A standard `for` that traverses a range of integer values can be written as follows:

```zephir
for i in range(1, 10) {
    echo i, "\n";
}
```

To avoid warnings about unused variables, you can use anonymous variables in `for` statements, by replacing a variable name with the placeholder `_`:

##### Use the key but ignore the value

```zephir
for key, _ in data {
    echo key, "\n";
}
```

<a name='loops-break'></a>

### Break Statement

`break` ends execution of the current `while`, `for` or `loop` statement:

```zephir
for item in ["a", "b", "c", "d"] {
    if item == "c" {
        break; // exit the for
    }
    echo item, "\n";
}
```

<a name='loops-continue'></a>

### Continue Statement

`continue` is used within looping structures to skip the rest of the current loop iteration and continue execution at the condition evaluation, and then the beginning of the next iteration.

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

## Require

The `require` statement dynamically includes and evaluates a specified PHP file. Note that files included via Zephir are interpreted by Zend Engine as normal PHP files. `require` does not allow Zephir code to include other Zephir files at runtime.

```zephir
if file_exists(path) {
    require path;
}
```

<a name='let'></a>

## Let

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