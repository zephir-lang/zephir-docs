---
layout: default
language: 'en'
version: '0.10'
menu:
  - text:
        'Some Features'
    url: '#some-features'
  - text:
        'A small taste'
    url: '#a-small-taste' 
---
# Welcome!
Welcome to Zephir, an open source, high-level/domain specific language designed to ease the creation and maintainability of extensions for PHP, with a focus on type and memory safety.

<a name='some-features'></a>
## Some features
Zephir's main features are:

| Feature            | Description                                           |
|--------------------|-------------------------------------------------------|
| Type system        | dynamic/static                                        |
| Memory safety      | pointers or direct memory management are not allowed  |
| Compilation model  | ahead of time                                         |
| Memory model       | task-local garbage collection                         |

<a name='a-small-taste'></a>
## A small taste
The following code registers a class with a method that filters variables, returning their alphabetic characters:

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

The class can be used from PHP as follows:

```php
<?php

$filter = new MyLibrary\Filter();
echo $filter->alpha("01he#l.lo?/1"); // prints hello
``` 

<a name='external-links'></a>
## External Links
Below we have collected links to external resources that may interest you:

- [Type system](https://en.wikipedia.org/wiki/Type_system)
- [Memory safety](https://en.wikipedia.org/wiki/Memory_safety)
- [Ahead-of-time compilation](https://en.wikipedia.org/wiki/Ahead-of-time_compilation)
- [Memory management](https://en.wikipedia.org/wiki/Memory_management)
