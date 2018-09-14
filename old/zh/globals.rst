扩展的全局变量
-----------------
Zephir书写的PHP扩展提供了一种定义全局配置变量的方式。读取或修改这些全局配置变量比其他的方式更快速（比如静态成员等)。
开发者可以用一些扩展的配置项变量来控制扩展的形为方式.

在Zephir中，扩展的全局变量类型只能严格的使用一些基本类型如int/bool/double/char等。
复杂的类型如string/arrays/objects/resources等是不支持的。

开发者可以在config.json文件中添加形如下的结构来打开全局配置变量:

.. code-block:: javascript

    {
        //...
        "globals": {
            "allow_some_feature": {
                "type": "bool",
                "default": true
            },
            "number_times": {
                "type": "int",
                "default": 10
            },
            "some_component.my_setting_1": {
                "type": "bool",
                "default": true
            },
            "some_component.my_setting_2": {
                "type": "int",
                "default": 100
            }
        }
    }

全局配置变量以如下的结构书写:

.. code-block:: javascript

    "<global-name>": {
        "type": "<some-valid-type>",
        "default": <some-compatible-default-value>
    }

组合全局配置变量使用如下的结构书写:

.. code-block:: javascript

    "<namespace>.<global-name>": {
        "type": "<some-valid-type>",
        "default": <some-compatible-default-value>
    }

在Zephir扩展的任何地方，开发者可以使用global_get与global_set来读写全局配置变量的值:

.. code-block:: zephir

    global_set("allow_some_feature", true);
    let someFeature = global_get("allow_some_feature");

如果开发者想在PHP端修改这些全局配置项，可以使用如下的方式来做:

.. code-block:: zephir

    namespace Test;

    class MyOptions
    {

        public static function setOptions(array options)
        {
            boolean someOption, anotherOption;

            if fetch someOption, options["some_option"] {
                globals_set("some_option", someOption);
            }

            if fetch anotherOption, options["another_option"] {
                globals_set("another_option", anotherOption);
            }
        }
    }

Zephir扩展的全局变量不能使用动态的方式来访问是因为global_get/global_set生成的代码是在编译时无法取得这些动态变量的值:

.. code-block:: zephir

    let myOption = "someOption";

    //这里会抛出编译异常
    let someOption = globals_get(myOption);
