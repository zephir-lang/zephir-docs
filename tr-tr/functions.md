---
layout: default
language: 'tr-tr'
version: '0.12'
---
# Calling Functions
PHP has a rich library of functions that you can use within your extensions. To call a PHP function you simply use it as normal within your Zephir code:

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

You can also call functions that are expected to exist in the PHP userland, but are not necessarily built in to PHP itself:

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

Note that all PHP functions only receive and return dynamic variables. If you pass a static typed variable as a parameter, a temporary dynamic variable will automatically be used as a bridge in order to call the function:

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

Similarly, functions return dynamic values, which cannot be directly assigned to static variables without the appropriate explicit cast:

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

Zephir also provides a way for you to call functions dynamically, such as:

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
