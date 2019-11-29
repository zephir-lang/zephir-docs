---
layout: default
language: 'en'
version: '0.10'
---

# 闭 包

您可以在Zephir中使用闭包(也称为匿名函数);这些是PHP兼容的，可以返回给PHP代码块:

```zephir
namespace MyLibrary;

class Functional
{

    public function map(array! data)
    {
        return function(number) {
            return number * number;
        };
    }
}
```

它也可以直接在Zephir中执行，并作为参数传递给其他函数/方法:

```zephir
namespace MyLibrary;

class Functional
{

    public function map(array! data)
    {
        return data->map(function(number) {
            return number * number;
        });
    }
}
```

一个简短的语法也可以用来定义闭包:

```zephir
namespace MyLibrary;

class Functional
{

    public function map(array! data)
    {
        return data->map(number => number * number);
    }
}
```
