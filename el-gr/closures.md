---
layout: default
language: 'el-gr'
version: '0.10'
---

# Closures

You can use closures (a.k.a. anonymous functions) in Zephir; these are PHP compatible and can be returned to the PHP userland:

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

It also can be executed directly within Zephir, and passed as a parameter to other functions/methods:

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

A short syntax is also available to define closures:

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
