---
layout: default
language: 'uk-ua'
version: '0.10'
menu:
  - text:
        'Some Features'
    url: '#some-features'
  - text:
        'A small taste'
    url: '#a-small-taste' 
---
# Вітаємо!

Вас вітає Zephir — проект з відкритим вихідним кодом, високорівнева/предметно-орієнтована мова спроектована для полегшення створення й супроводу розширень для PHP з акцентом на тип та безпеку доступу до пам'яті.

<a name='some-features'></a>

## Деякі особливості

Основними особливостями Zephir-у є:

| Особливість                 | Description                                        |
| --------------------------- | -------------------------------------------------- |
| Система типізації           | динамічна/статична                                 |
| Безпечний доступ до пам'яті | вказівники або пряме керування пам'яттю заборонені |
| Компіляційна модель         | компіляція виконується заздалегідь                 |
| Модель пам'яті              | свій збирач сміття                                 |

<a name='a-small-taste'></a>

## Скуштуйте

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

## Зовнішні посилання

Нижче ми зібрали посилання на зовнішні ресурси, які можуть вас зацікавити:

- [Система типізації](https://en.wikipedia.org/wiki/Type_system)
- [Безпечний доступ до пам'яті](https://en.wikipedia.org/wiki/Memory_safety)
- [Ahead-of-time (AOT) компіляція](https://en.wikipedia.org/wiki/Ahead-of-time_compilation)
- [Керування пам’яттю](https://en.wikipedia.org/wiki/Memory_management)
