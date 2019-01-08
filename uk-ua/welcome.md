* * *

layout: default language: 'en' version: '0.10'

* * *

# Welcome!

Вас вітає Zephir — проект з відкритим вихідним кодом, високорівнева/предметно-орієнтована мова спроектована для полегшення створення й супроводу розширень для PHP з акцентом на тип та безпеку доступу до пам'яті.

<a name='some-features'></a>

## Some features

Основними особливостями Zephir-у є:

| Feature           | Description                                          |
| ----------------- | ---------------------------------------------------- |
| Type system       | dynamic/static                                       |
| Memory safety     | pointers or direct memory management are not allowed |
| Compilation model | ahead of time                                        |
| Memory model      | task-local garbage collection                        |

<a name='a-small-taste'></a>

## A small taste

Наступний код реєструє клас з методом, який фільтрує змінні, повертаючи лише їхні алфавітні символи:

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

Цей клас можна виконати з PHP наступним чином:

```php
<?php

$filter = new MyLibrary\Filter();
echo $filter->alpha("01he#l.lo?/1"); // prints hello
```

<a name='external-links'></a>

## External Links

Нижче ми зібрали посилання на зовнішні ресурси, які можуть вас зацікавити:

- [Type system](https://en.wikipedia.org/wiki/Type_system)
- [Memory safety](https://en.wikipedia.org/wiki/Memory_safety)
- [Ahead-of-time (AOT) компіляція](https://en.wikipedia.org/wiki/Ahead-of-time_compilation)
- [Керування пам’яттю](https://en.wikipedia.org/wiki/Memory_management)