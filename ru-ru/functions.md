---
layout: default
language: 'ru-ru'
version: '0.11'
---

# Вызов функций
PHP имеет богатую библиотеку функций, которые вы можете использовать в своих расширениях. Чтобы вызвать функцию PHP, вы просто используете её как обычно в своем коде Zephir:

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

Вы также можете вызывать функции, которые, как ожидается, существуют в пользовательском окружении PHP, но не встроены в PHP:

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

Обратите внимание, что все PHP-функции только получают и возвращают динамические переменные. Если вы передаете статически типизированную переменную в качестве параметра, то в качестве моста для вызова функции будет создана временная динамическая переменная:

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

Аналогично, функции возвращают динамические значения, которые не могут быть напрямую назначены статическим переменным без соответствующего приведения:

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

Zephir также предоставляет способ для динамического вызова функций, например:

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
