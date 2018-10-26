# Добро пожаловать!

Встречайте Zephir, открытый, высокоуровневый, специализированный язык разработанный для быстрого и удобного создания расширений для PHP с упором на типизацию и безопасное управление памятью.

<a name='some-features'></a>

## Некоторые особенности

Основные особенности Zephir:

| Особенность                   | Описание                                      |
| ----------------------------- | --------------------------------------------- |
| Система типов                 | динамическая/статическая                      |
| Безопасное управление памятью | указатели и ручное выделение памяти запрещены |
| Модель компиляции             | ahead of time                                 |
| Memory model                  | task-local garbage collection                 |

<a name='a-small-taste'></a>

## A small taste

The following code registers a class with a method that filters variables, returning their alphabetic characters:

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
    

The class can be used from PHP as follows:

    <?php
    
    $filter = new MyLibrary\Filter();
    echo $filter->alpha("01he#l.lo?/1"); // prints hello