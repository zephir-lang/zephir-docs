Вызов функций
-------------
PHP имеет богатую библиотеку функций, которые вы можете использовать в своих расширениях. 
Чтобы вызвать функцию PHP, вы просто используете ее как обычно в своем коде Zephir:

.. code-block:: zephir

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

Вы также можете вызывать функции, которые, как ожидается, существуют в пользовательском окружении PHP, но не встроены в PHP:

.. code-block:: zephir

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

Обратите внимание, что все функции PHP только получают и возвращают динамические переменные. 
Если вы передаете переменную статического типа  в качестве параметра, то  в качестве моста для вызова функции будет создана
временная динамическая переменная:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(string text)
        {
            if strlen(text) != 0 {
                // Неявная динамическая переменная создается, 
                // чтобы передать переменную статического типа 
                // 'text' в качестве параметра
                return base64_encode(text);
            }
            return false;
        }
    }

Аналогично, функции возвращают динамические значения, которые не могут быть напрямую назначены статическим 
переменным без соответствующего приведения:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(string text)
        {
            string encoded = "";

            if strlen(text) != 0 {
                let encoded = (string) base64_encode(text);
                return '(' . encoded . ')';
            }
            return false;
        }
    }

Zephir предоставляет вам возможность динамически вызывать функции, такие как:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(var callback, string text)
        {
            return {callback}(text);
        }
    }
