* * *

layout: default language: 'en' version: '0.11' menu:

- text: 'String' url: '#string'
- text: 'Array' url: '#array'
- text: 'Char' url: '#char'
- text: 'Integer' url: '#integer'

* * *

# 内建方法

如前所述，Zephir提倡面向对象编程。 与静态类型相关的变量也可以作为对象处理。

比较这两种方法:

```zephir
public function binaryToHex(string! s) -> string
{
    var o = "", n; char ch;

    for ch in range(0, strlen(s)) {
        let n = sprintf("%X", ch);
        if strlen(n) < 2 {
            let o .= "0" . n;
        } else {
            let o .= n;
        }
    }
    return o;
}
```

与

```zephir
public function binaryToHex(string! s) -> string
{
    var o = "", n; char ch;

    for ch in range(0, s->length()) {
        let n = ch->toHex();
        if n->length() < 2 {
            let o .= "0" . n;
        } else {
            let o .= n;
        }
    }
    return o;
}
```

它们都有相同的功能，但是第二个使用的是面向对象编程。 对静态类型变量调用方法对性能没有任何影响，因为Zephir在内部将代码从面向对象版本转换为过程版本。

<a name='string'></a>

## String

下面的字符串内置方法可用:

| OO                   | 实际过程                  | 说明                     |
| -------------------- | --------------------- | ---------------------- |
| `s->format()`     | `sprintf(s, "%s", x)` | 返回格式化的字符串              |
| `s->index("foo")` | `strpos(s, "foo")`    | 查找字符串中第一个出现的子字符串的位置    |
| `s->length()`     | `strlen(s)`           | 获取字符串长度                |
| `s->lower()`      | `strtolower(s)`       | 使字符串小写                 |
| `s->lowerfirst()` | `lcfirst(s)`          | 使字符串的第一个字符小写           |
| `s->md5()`        | `md5(s)`              | 计算字符串的 md5 哈希          |
| `s->sha1()`       | `sha1(s)`             | 计算字符串的 sha1 哈希         |
| `s->trim()`       | `trim(s)`             | 删除字符串的开头和结尾的空格 (或其他字符) |
| `s->trimleft()`   | `ltrim(s)`            | 从字符串开头的条带空白 (或其他字符)    |
| `s->trimright()`  | `rtrim(s)`            | 删除字符串末端的空白字符（或者其他字符）   |
| `s->upper()`      | `strtoupper(s)`       | 使字符串大写                 |
| `s->upperfirst()` | `ucfirst(s)`          | 使字符串的第一个字符大写           |

<a name='array'></a>

## Array

可用的数组内置方法如下:

| OO                   | 实际过程                    | 说明                      |
| -------------------- | ----------------------- | ----------------------- |
| `a->combine(b)`   | `array_combine(a, b)`   | 通过使用一个数组表示键, 为其值创建另一个数组 |
| `a->diff()`       | `array_diff(a)`         | 计算数组的差异                 |
| `a->flip()`       | `array_flip(a)`         | 将数组中的所有键与其关联的值交换        |
| `a->hasKey()`     | `array_key_exists(a)`   | 检查数组中是否存在给定的键或索引        |
| `a->intersect(b)` | `array_intersect(a, b)` | 计算数组的交集                 |
| `a->join(" ")`    | `join(" ", a)`          | 使用字符串联接数组元素             |
| `a->keys()`       | `array_keys(a)`         | 返回数组的所有键或键的子集           |
| `a->merge(b)`     | `array_merge(a, b)`     | 合并一个或多个数组               |
| `a->pad()`        | `array_pad(a, b)`       | 以指定长度将一个值填充进数组          |
| `a->rev()`        | `array_reverse(a)`      | 返回具有相反顺序的元素的数组          |
| `a->reversed()`   | `array_reverse(a)`      | 返回具有相反顺序的元素的数组          |
| `a->split()`      | `array_chunk(a)`        | 将数组拆分为多个块               |
| `a->values()`     | `array_values(a)`       | 返回数组的所有值                |
| `a->walk()`       | `array_walk(a)`         | 使用用户自定义函数对数组中的每个元素做回调处理 |

<a name='char'></a>

## Char

提供了以下字符内置方法:

| OO               | 实际过程                |
| ---------------- | ------------------- |
| `ch->toHex()` | `sprintf("%X", ch)` |

<a name='integer'></a>

## Integer

以下是可用的整数内置方法:

| OO            | 实际过程     |
| ------------- | -------- |
| `i->abs()` | `abs(i)` |