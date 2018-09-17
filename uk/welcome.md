# Вітаємо!

Вас вітає Zephir — проект з відкритим вихідним кодом, високорівнева/предметно-орієнтована мова спроектована для полегшення створення й супроводу розширень для PHP з акцентом на тип та безпеку доступу до пам'яті.

<a name='some-features'></a>

## Деякі особливості

Основними особливостями Zephir-у є:

| Особливість       | Description                                          |
| ----------------- | ---------------------------------------------------- |
| Type system       | dynamic/static                                       |
| Memory safety     | pointers or direct memory management are not allowed |
| Compilation model | ahead of time                                        |
| Memory model      | task-local garbage collection                        |

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