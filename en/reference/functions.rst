Functions
---------
PHP has a rich library of functions you can use in your extensions.
To call a PHP function you can just refer its name in the Zephir code:

.. code-block:: javascript

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
