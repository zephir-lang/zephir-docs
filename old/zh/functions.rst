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

Zephir中可以使用PHP中定义而Zephir中未定义的函数:

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

注意我们在Zephir中使用PHP的函数只接受动态变量参数，如果我们传了一个静态类型变量作为参数，则Zephir的编译器会
生成一个动态的中间变量作为桥梁来调用这个PHP函数:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(string text)
        {
            if strlen(text) != 0 {
                //由于text是静态变量，所以Zephir的编译器会隐式的生成一个动态变量来传递给如下的函数来完成调用
                return base64_encode(text);
            }
            return false;
        }

    }

同样的，函数返回的动态变量也不能直接传给静态变量，需要进行适当的强制转换:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(string text)
        {
            string encoded = "";

            if strlen(text) != 0 {
                let encoded = (string) base64_encode(text);
                return "(" . encoded . ")";
            }
            return false;
        }

    }

有时我们用如下方式来动态的调用函数，

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(var callback, string text)
        {
            return {callback}(text);
        }

    }

