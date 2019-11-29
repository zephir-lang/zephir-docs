* * *

layout: default language: 'en' version: '0.10'

* * *

# 数组

Zephir中的数组操作提供了一种类似PHP [数组](http://www.php.net/manual/en/language.types.array.php)的方法。 数组是 < 0>hash 表 </0 > 的实现。

<a name='declaring-array-variables'></a>

## 声明数组变量

数组变量可以使用关键字 "var" 或 "array" 来声明:

```zephir
var a   = []; // 数组变量，其类型可以改变
array b = []; // 数组变量，其类型不能在执行过程中更改
```

<a name='creating-arrays'></a>

## 创建数组

通过将其元素括在方括号中创建数组:

##### 创建空数组

```zephir
let elements = [];
```

##### 创建包含元素的数组

```zephir
let elements = [1, 3, 4];
```

##### 使用不同类型的元素创建数组

```zephir
let elements = ["first", 2, true];
```

##### 多维数组

```zephir
let elements = [[0, 1], [4, 5], [2, 3]];
```

就像 PHP，哈希或字典都支持的:

##### 使用字符串键创建哈希

```zephir
let elements = ["foo": "bar", "bar": "foo"];
```

##### 使用数字键创建哈希

```zephir
let elements = [4: "bar", 8: "foo"];
```

##### 使用字符串和数字键混合创建哈希

```zephir
let elements = [4: "bar", "foo": 8];
```

<a name='updating-arrays'></a>

## Updating arrays

Arrays are updated in the same way as PHP, using square brackets:

##### 使用字符串键名更新数组

```zephir
let elements["foo"] = "bar";
```

##### 使用数字键更新数组

```zephir
let elements[0] = "bar";
```

##### 多维数组

```zephir
let elements[0]["foo"] = "bar";
let elements["foo"][0] = "bar";
```

<a name='appending-elements'></a>

## 追加元素

元素可以追加到数组的末尾, 如下所示:

##### 向数组追加一个元素

```zephir
let elements[] = "bar";
```

<a name='reading-elements-from-arrays'></a>

## 从数组中读取元素

可以读取数组元素, 如下所示:

##### 使用字符串键获取元素 `foo`

```zephir
let foo = elements["foo"];
```

##### 使用数字键0获取元素

```zephir
let foo = elements[0];
```