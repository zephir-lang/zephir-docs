---
layout: default
language: 'ru-ru'
version: '0.11'
---

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

Они также могут быть выполнены непосредственно в Zephir, и переданы в качестве параметра другим функциям/методам:

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

Для определения замыканий доступен также короткий синтаксис:

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
