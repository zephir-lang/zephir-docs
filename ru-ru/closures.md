* * *

layout: default language: 'ru-ru' version: '0.11'

* * *

# Замыкания

В Zephir вы можете использовать анонимные функции (также известные как замыкания); Они совместимы с PHP и могут передаваться в область видимости PHP кода:

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