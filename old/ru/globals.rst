Глобальные параметры рассширения
--------------------------------
Расширения PHP предоставляют способ определения глобальных переменных внутри расширения. 
Чтение/запись глобальных данных должны быть быстрее, чем любые другие глобальные механизмы (например, статические члены).
Глобальные значения расширений можно использовать для настройки параметров конфигурации, 
которые изменяют поведение вашей библиотеки.

В Zephir расширения globals ограничены простыми скалярными типами типа int/bool/double/char и т.д. 
Здесь не допускаются сложные типы, такие как строки/массивы/объекты/ресурсы.

Вы можете включить глобальные расширения, добавив следующую структуру в config.json:

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

Каждая глобальная секция имеет следующую структуру:

.. code-block:: javascript

    "<global-name>": {
        "type": "<some-valid-type>",
        "default": <some-compatible-default-value>
    }

Составные глобальные переменные имеют следующую структуру:

.. code-block:: javascript

    "<namespace>.<global-name>": {
        "type": "<some-valid-type>",
        "default": <some-compatible-default-value>
    }

Внутри любого метода вы можете читать/писать глобальные расширения с помощью встроенных функций  globals_get/globals_set:

.. code-block:: zephir

    globals_set("allow_some_feature", true);
    let someFeature = globals_get("allow_some_feature");

Если вы хотите изменить эти глобальные переменные из PHP, хорошим вариантом является включение метода, направленного на это:

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

Расширения globals не могут быть динамически доступны, так как C-код, сгенерированный оптимизаторами globals_get / globals_set, должен быть разрешен во время компиляции:

.. code-block:: zephir

    let myOption = "someOption";

    // Будет генерировать исключение компилятора
    let someOption = globals_get(myOption);
