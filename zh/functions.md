# 调用函数

PHP有一个丰富的函数库，您可以在扩展中使用它们。 要调用PHP函数，只需在Zephir代码中正常使用它：

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
    

您还可以调用预期存在于PHP用户区中的函数，但不一定是内置于PHP本身：

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
    

请注意，所有PHP函数仅接收和返回动态变量。 If you pass a static typed variable as a parameter, a temporary dynamic variable will automatically be used as a bridge in order to call the function:

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
    

Similarly, functions return dynamic values, which cannot be directly assigned to static variables without the appropriate explicit cast:

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
    

Zephir also provides a way for you to call functions dynamically, such as:

    namespace MyLibrary;
    
    class Encoder
    {
        public function encode(var callback, string text)
        {
            return {callback}(text);
        }
    }