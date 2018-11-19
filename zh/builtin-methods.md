# 内建方法

如前所述，Zephir提倡面向对象编程。 与静态类型相关的变量也可以作为对象处理。

比较这两种方法:

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
    

与

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
    

它们都有相同的功能，但是第二个使用的是面向对象编程。 对静态类型变量调用方法对性能没有任何影响，因为Zephir在内部将代码从面向对象版本转换为过程版本。

<a name='string'></a>

## String

下面的字符串内置方法可用:

| OO                   | 实际过程                  | 说明                                 |
| -------------------- | --------------------- | ---------------------------------- |
| `s->format()`     | `sprintf(s, "%s", x)` | 返回格式化的字符串                          |
| `s->index("foo")` | `strpos(s, "foo")`    | 查找字符串中第一个出现的子字符串的位置                |
| `s->length()`     | `strlen(s)`           | 获取字符串长度                            |
| `s->lower()`      | `strtolower(s)`       | 使字符串小写                             |
| `s->lowerfirst()` | `lcfirst(s)`          | 使字符串的第一个字符小写                       |
| `s->md5()`        | `md5(s)`              | Calculate the md5 hash of a string |
| `s->sha1()`       | `sha1(s)`             | 计算字符串的 sha1 哈希                     |
| `s->trim()`       | `trim(s)`             | 删除字符串的开头和结尾的空格 (或其他字符)             |
| `s->trimleft()`   | `ltrim(s)`            | 从字符串开头的条带空白 (或其他字符)                |
| `s->trimright()`  | `rtrim(s)`            | 删除字符串末端的空白字符（或者其他字符）               |
| `s->upper()`      | `strtoupper(s)`       | 使字符串大写                             |
| `s->upperfirst()` | `ucfirst(s)`          | 使字符串的第一个字符大写                       |

<a name='array'></a>

## 数组

可用的数组内置方法如下:

| OO                   | 实际过程                    | 说明                                                         |
| -------------------- | ----------------------- | ---------------------------------------------------------- |
| `a->combine(b)`   | `array_combine(a, b)`   | 通过使用一个数组表示键, 为其值创建另一个数组                                    |
| `a->diff()`       | `array_diff(a)`         | 计算数组的差异                                                    |
| `a->flip()`       | `array_flip(a)`         | 将数组中的所有键与其关联的值交换                                           |
| `a->hasKey()`     | `array_key_exists(a)`   | 检查数组中是否存在给定的键或索引                                           |
| `a->intersect(b)` | `array_intersect(a, b)` | 计算数组的交集                                                    |
| `a->join(" ")`    | `join(" ", a)`          | 使用字符串联接数组元素                                                |
| `a->keys()`       | `array_keys(a)`         | 返回数组的所有键或键的子集                                              |
| `a->merge(b)`     | `array_merge(a, b)`     | Merge one or more arrays                                   |
| `a->pad()`        | `array_pad(a, b)`       | Pad array to the specified length with a value             |
| `a->rev()`        | `array_reverse(a)`      | Return an array with elements in reverse order             |
| `a->reversed()`   | `array_reverse(a)`      | Return an array with elements in reverse order             |
| `a->split()`      | `array_chunk(a)`        | Split an array into chunks                                 |
| `a->values()`     | `array_values(a)`       | Return all the values of an array                          |
| `a->walk()`       | `array_walk(a)`         | Apply a user supplied function to every member of an array |

<a name='char'></a>

## Char

The following char built-in methods are available:

| OO               | 实际过程                |
| ---------------- | ------------------- |
| `ch->toHex()` | `sprintf("%X", ch)` |

<a name='integer'></a>

## Integer

The following integer built-in methods are available:

| OO            | 实际过程     |
| ------------- | -------- |
| `i->abs()` | `abs(i)` |