---
layout: default
language: 'ru-ru'
version: '0.10'
---
# Добро пожаловать!

Встречайте Zephir, открытый, высокоуровневый, специализированный язык разработанный для быстрого и удобного создания расширений для PHP с упором на типизацию и безопасное управление памятью.

<a name='some-features'></a>

## Некоторые особенности

Основные особенности Zephir:

| Особенность                   | Описание                                      |
| ----------------------------- | --------------------------------------------- |
| Система типов                 | динамическая/статическая                      |
| Безопасность доступа к памяти | указатели и ручное выделение памяти запрещены |
| Модель компиляции             | перед исполнением (AOT-компиляция)            |
| Управление памятью            | свой сборщик мусора                           |

<a name='a-small-taste'></a>

## Попробуйте

Этот код регистрирует класс с методом, который оставляет в строке только буквы:

```zephir
namespace MyLibrary;

/**
 * Filter
 */
class Filter
{
    /**
     * Filters a string, returning its alpha charactersa
     *
     * @param string str
     */
    public function alpha(string str)
    {
        char ch; string filtered = "";

        for ch in str {
           if (ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z') {
              let filtered .= ch;
           }
        }

        return filtered;
    }
}
```

А теперь используем этот класс в PHP:

```php
<?php

$filter = new MyLibrary\Filter();
echo $filter->alpha("01he#l.lo?/1"); // выведет hello
```

<a name='external-links'></a>

## Внешние ссылки

Ниже мы собрали ссылки на внешние ресурсы, которые могу вас заинтересовать:

- [Система типов](https://en.wikipedia.org/wiki/Type_system)
- [Безопасность доступа к памяти](https://en.wikipedia.org/wiki/Memory_safety)
- [AOT-компиляция](https://en.wikipedia.org/wiki/Ahead-of-time_compilation)
- [Управление памятью](https://en.wikipedia.org/wiki/Memory_management)
