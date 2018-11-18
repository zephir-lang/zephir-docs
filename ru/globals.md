# Глобальные параметры расширения

Расширения PHP предоставляют способ определения глобальных параметров внутри расширения. Чтение/запись глобальных данных должны быть быстрее, чем любые другие глобальные механизмы (например, доступ к статическим членам). Глобальные параметры расширений можно использовать для настройки параметров конфигурации, которые изменяют поведение вашей библиотеки.

В Zephir глобальные параметры ограничены простыми скалярными типами типа `int`/`bool`/`double`/`char` и т.д. Здесь не допускаются комплексные типы, такие как строки/массивы/объекты/ресурсы.

Вы можете ввести глобальные параметры в расширение, добавив следующую структуру в `config.json`:

    {
        //...
        "globals": {
            "allow_some_feature": {
                "type": "bool",
                "default": true,
                "module": true
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

    "<global-name>": {
        "type": "<some-valid-type>",
        "default": <some-compatible-default-value>
    }
    

Составные (именованные) глобальные параметры имеют следующую структуру:

    "<namespace>.<global-name>": {
        "type": "<some-valid-type>",
        "default": <some-compatible-default-value>
    }
    

The optional `module` key, if present, places that global's initialization process into the module-wide `GINIT` lifecycle event, which just means it will only be set up once per PHP process, rather than being reinitialized for every request, which is the default:

##### `allow_some_feature"` is set up only once at startup; `number_times` is set up at the start of each request

    {
        //...
        "globals": {
            "allow_some_feature": {
                "type": "bool",
                "default": true,
                "module": true
            },
            "number_times": {
                "type": "int",
                "default": 10
            }
        }
    }
    

Внутри любого метода вы можете читать/писать глобальные параметры расширения с помощью встроенных функций `globals_get` и `globals_set`:

    globals_set("allow_some_feature", true);
    let someFeature = globals_get("allow_some_feature");
    

Если вы хотите изменить эти глобальные параметры из PHP, неплохим вариантом является создание метода, направленного на это:

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
    

Доступ к глобальным параметрам расширения не может быть осуществлен с помощью динамических переменных, так как C-код, сгенерированный оптимизаторами `globals_get` и `globals_set`, должен быть вычислен во время компиляции:

    let myOption = "someOption";
    
    // Выбросит ошибку компиляции
    let someOption = globals_get(myOption);