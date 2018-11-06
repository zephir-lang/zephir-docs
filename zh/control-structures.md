# 控制结构

Zephir实现了一组简化的控制结构，这些结构用类似的语言表示，如C、PHP等。

<a name='conditionals'></a>

## 条件

<a name='conditionals-if'></a>

### If 语句

`if`语句对一个表达式求值，如果求值为true，则执行以下代码块。 括号是必需的。 一个`如果`可以有一个可选的`else`子句, 而且多个`if`/`else`构造可以放在一起 

    if false {
        echo "false?";
    } else {
        if true {
            echo "true!";
        } else {
            echo "neither true nor false";
        }
    }
    

`elseif`也有条件

    if a > 100 {
        echo "to big";
    } elseif a < 0 {
        echo "to small";
    } elseif a == 50 {
        echo "perfect!";
    } else {
        echo "ok";
    }
    

计算表达式中的括号是可选的:

    if a < 0 { return -1; } else { if a > 0 { return 1; } }
    

<a name='conditionals-switch'></a>

### Switch 语句

一个`switch`根据一系列预定义的文字值计算表达式，执行相应的`case`块或回落到`default`块:

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
    

<a name='loops'></a>

## Loops

<a name='loops-while'></a>

### While 语句

`while` 表示循环, 只要其给定条件的计算结果为 true, 该循环就会迭代到:

    let counter = 5;
    while counter {
        let counter -= 1;
    }
    

<a name='loops-loop'></a>

### Loop 语句

除了 `while`, `loop` 还可用于创建无限循环:

    let n = 40;
    loop {
        let n -= 2;
        if n % 5 == 0 { break; }
        echo x, "\n";
    }
    

<a name='loops-for'></a>

### For 语句

`for` 是一种控制结构, 允许遍历数组或字符串:

    for item in ["a", "b", "c", "d"] {
        echo item, "\n";
    }
    

可以通过为键和值提供变量来获取哈希中的键：

    let items = ["a": 1, "b": 2, "c": 3, "d": 4];
    
    for key, value in items {
        echo key, " ", value, "\n";
    }
    

A `for` loop can also be instructed to traverse an array or string in reverse order:

    let items = [1, 2, 3, 4, 5];
    
    for value in reverse items {
        echo value, "\n";
    }
    

A `for` loop can be used to traverse string variables:

    string language = "zephir"; char ch;
    
    for ch in language {
        echo "[", ch ,"]";
    }
    

In reverse order:

    string language = "zephir"; char ch;
    
    for ch in reverse language {
        echo "[", ch ,"]";
    }
    

A standard `for` that traverses a range of integer values can be written as follows:

    for i in range(1, 10) {
        echo i, "\n";
    }
    

To avoid warnings about unused variables, you can use anonymous variables in `for` statements, by replacing a variable name with the placeholder `_`:

##### Use the key but ignore the value

    for key, _ in data {
        echo key, "\n";
    }
    

<a name='loops-break'></a>

### Break Statement

`break` ends execution of the current `while`, `for` or `loop` statement:

    for item in ["a", "b", "c", "d"] {
        if item == "c" {
            break; // exit the for
        }
        echo item, "\n";
    }
    

<a name='loops-continue'></a>

### Continue Statement

`continue` is used within looping structures to skip the rest of the current loop iteration and continue execution at the condition evaluation, and then the beginning of the next iteration.

    let a = 5;
    while a > 0 {
        let a--;
        if a == 3 {
            continue;
        }
        echo a, "\n";
    }
    

<a name='require'></a>

## Require

The `require` statement dynamically includes and evaluates a specified PHP file. Note that files included via Zephir are interpreted by Zend Engine as normal PHP files. `require` does not allow Zephdr code to include other Zephir files at runtime.

    if file_exists(path) {
        require path;
    }
    

<a name='let'></a>

## Let

The `let` statement is used to mutate variables, properties and arrays. Variables are by default immutable and this instruction makes them mutable for the duration of the statement:

    let name = "Tony";           // simple variable
    let this->name = "Tony";     // object property
    let data["name"] = "Tony";   // array index
    let self::_name = "Tony";    // static property
    

Also this instruction must be used to increment/decrement variables:

    let number++;           // increment simple variable
    let number--;           // decrement simple variable
    let this->number++;     // increment object property
    let this->number--;     // decrement object property
    

Multiple mutations can be performed in a single `let` operation:

    let price = 1.00, realPrice = price, status = false;