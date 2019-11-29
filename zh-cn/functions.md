---
layout: default
language: 'zh-cn'
version: '0.11'
---

# 调用函数

PHP有一个丰富的函数库，您可以在扩展中使用它们。 要调用PHP函数，只需在Zephir代码中正常使用它：

```zephir
namespace MyLibrary;

class Encoder
{
    public function encode(var text)
    {
        if strlen(text) != 0 {
            return base64_encode(text);
        }
        return false;
    }
}
```

您还可以调用预期存在于PHP用户区中的函数，但不一定是内置于PHP本身：

```zephir
namespace MyLibrary;

class Encoder
{

    public function encode(var text)
    {
        if strlen(text) != 0 {
            if function_exists("my_custom_encoder") {
                return my_custom_encoder(text);
            } else {
                return base64_encode(text);
            }
        }
        return false;
    }
}
```

请注意，所有PHP函数仅接收和返回动态变量。 如果将静态类型变量作为参数传递，则临时动态变量将自动用作桥接器以调用该函数：

```zephir
namespace MyLibrary;

class Encoder
{
    public function encode(string text)
    {
        if strlen(text) != 0 {
            // an implicit dynamic variable is created to
            // pass the static typed 'text' as parameter
            return base64_encode(text);
        }
        return false;
    }
}
```

类似地，函数返回动态值，如果没有适当的显式强制转换，则无法直接将其分配给静态变量：

```zephir
namespace MyLibrary;

class Encoder
{
    public function encode(string text)
    {
        string encoded = "";

        if strlen(text) != 0 {
            let encoded = (string) base64_encode(text);
            return "(" . encoded . ")";
        }
        return false;
    }
}
```

Zephir还为您提供了一种动态调用函数的方法，例如：

```zephir
namespace MyLibrary;

class Encoder
{
    public function encode(var callback, string text)
    {
        return {callback}(text);
    }
}
```
