# Вітаємо!

Вас вітає Zephir — проект з відкритим вихідним кодом, високорівнева/предметно-орієнтована мова спроектована для полегшення створення й супроводу розширень для PHP з акцентом на тип та безпеку доступу до пам'яті.

<a name='some-features'></a>

## Деякі особливості

Основними особливостями Zephir-у є:

| Особливість                 | Description                                             |
| --------------------------- | ------------------------------------------------------- |
| Система типізації           | динамічна/статична                                      |
| Безпечний доступ до пам'яті | вказівники або пряме керування пам'яттю не допускаються |
| Компіляційна модель         | ahead of time                                           |
| Memory model                | task-local garbage collection                           |

<a name='a-small-taste'></a>

## Скуштуйте

Наступний код реєструє клас з методом, який фільтрує змінні, повертаючи лише їхні алфавітні символи:

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
    

Цей клас можна виконати з PHP наступним чином:

    <?php
    
    $filter = new MyLibrary\Filter();
    echo $filter->alpha("01he#l.lo?/1"); // prints hello