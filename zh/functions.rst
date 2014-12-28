调用函数
-----------------
你可以在你的扩展中使用PHP丰富的函数库。
你可以参考下面的代码调用一个函数。

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

You can call also functions that are expected to exist in the PHP userland but they
aren't built-in with PHP:

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

Note that all PHP functions only receive and return dynamic variables, if you pass a static typed
variable as a parameter, some temporary dynamic variable will be used as a bridge in order to call them:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(string text)
        {
            if strlen(text) != 0 {
                // an implicit dynamic variable is created to
                // pass the static typed 'text' as parameter
                return base64_encode(text);
            }
            return false;
        }

    }

Similarly, functions return dynamic values that cannot be directly assigned to static
variables without the appropriate cast:

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

Sometimes, we would need to call functions in a dynamic way, you can call them as follows:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(var callback, string text)
        {
            return {callback}(text);
        }

    }

