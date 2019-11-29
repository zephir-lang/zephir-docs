---
layout: default
language: 'en'
version: '0.12'
---

# 控制结构

Zephir实现了一组简化的控制结构，这些结构用类似的语言表示，如C、PHP等。

<a name='conditionals'></a>

## 条件

<a name='conditionals-if'></a>

### If 语句

`if` statements evaluate an expression, executing the following block if the evaluation is `true`. 括号是必需的。 一个`如果`可以有一个可选的`else`子句, 而且多个`if`/`else`构造可以放在一起 

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

`elseif`也有条件

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

计算表达式中的括号是可选的:

```zephir
if a < 0 { return -1; } else { if a > 0 { return 1; } }
```

<a name='conditionals-switch'></a>

### Switch 语句

一个`switch`根据一系列预定义的文字值计算表达式，执行相应的`case`块或回落到`default`块:

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

### While 语句

`while` denotes a loop that iterates as long as its given condition evaluates as `true`:

```zephir
let counter = 5;
while counter {
    let counter -= 1;
}
```

<a name='loops-loop'></a>

### Loop 语句

除了 `while`, `loop` 还可用于创建无限循环:

```zephir
let n = 40;
loop {
    let n -= 2;
    if n % 5 == 0 { break; }
    echo x, "\n";
}
```

<a name='loops-for'></a>

### For 语句

`for` 是一种控制结构, 允许遍历数组或字符串:

```zephir
for item in ["a", "b", "c", "d"] {
    echo item, "\n";
}
```

可以通过为键和值提供变量来获取哈希中的键：

```zephir
let items = ["a": 1, "b": 2, "c": 3, "d": 4];

for key, value in items {
    echo key, " ", value, "\n";
}
```

还可以指示` for </ 0>循环以相反的顺序遍历数组或字符串：</p>

<pre><code class="zephir">let items = [1, 2, 3, 4, 5];

for value in reverse items {
    echo value, "\n";
}
`</pre> 

` for </ 0>循环可用于遍历字符串变量：</p>

<pre><code class="zephir">string language = "zephir"; char ch;

for ch in language {
    echo "[", ch ,"]";
}
`</pre> 

按相反顺序：

```zephir
string language = "zephir"; char ch;

for ch in reverse language {
    echo "[", ch ,"]";
}
```

遍历一系列整数值` `可以写成如下：

```zephir
for i in range(1, 10) {
    echo i, "\n";
}
```

要避免对未使用的变量发出警告，可以在`for`语句中使用匿名变量，方法是使用占位符` _ </ 0>替换变量名称：</p>

<h5>使用键, 但忽略该值</h5>

<pre><code class="zephir">for key, _ in data {
    echo key, "\n";
}
`</pre> 

<a name='loops-break'></a>

### Break 语句

`break` 结束当前 `while`、`for` 或 `loop` 语句的执行:

```zephir
for item in ["a", "b", "c", "d"] {
    if item == "c" {
        break; // exit the for
    }
    echo item, "\n";
}
```

<a name='loops-continue'></a>

### Continue 语句

在循环结构中使用 `continue` 跳过当前循环迭代的其余部分, 并在条件计算时继续执行, 然后在下一次迭代的开始时继续执行。

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

` require </ 0>语句动态地包含和执行指定的PHP文件。 请注意，Zend Engine将Zephir包含的文件解释为普通的PHP文件。 <code>require` does not allow Zephir code to include other Zephir files at runtime.

```zephir
if file_exists(path) {
    require path;
}
```

<a name='let'></a>

## Let

`let` 语句用于可变变量、属性和数组。 变量默认是不可变的，并且该指令使它们在语句的作用域内是可变的：

```zephir
let name = "Tony";           // simple variable
let this->name = "Tony";     // object property
let data["name"] = "Tony";   // array index
let self::_name = "Tony";    // static property
```

此指令也必须用于递增/递减变量：

```zephir
let number++;           // increment simple variable
let number--;           // decrement simple variable
let this->number++;     // increment object property
let this->number--;     // decrement object property
```

可以在单个` let </ 0>操作中执行多个突变：</p>

<pre><code class="zephir">let price = 1.00, realPrice = price, status = false;
`</pre>