Конфигурационный файл
=====================
Каждое расширение Zephir имеет файл конфигурации, называемый config.json. Этот файл читается Zephir каждый раз, 
когда вы создаете или создаете расширение, и это позволяет разработчику изменять расширение или поведение компилятора.

Этот файл использует формат `JSON <https://ru.wikipedia.org/wiki/JSON>`_ в качестве формата конфигурации:

.. code-block:: json

    {
        "namespace": "test",
        "name": "Test Extension",
        "description": "My amazing extension",
        "author": "Tony Hawk",
        "version": "1.2.0"
    }

Параметры, определенные в этом файле, переопределяют любые заводские настройки, предоставляемые Zephir.

Поддерживаются следующие параметры:

namespace
^^^^^^^^^
Пространство имен расширения - это должен быть простой идентификатор, соответствующий регулярному выражению: [a-zA-Z0-9\_]+:

.. code-block:: json

    {
        "namespace": "test"
    }

name
^^^^
Имя расширения может содержать только символы ascii:

.. code-block:: json

    {
        "namespace": "test"
    }

description
^^^^^^^^^^^
Расширение описание, любой текст, описывающий расширение:

.. code-block:: json

    {
        "description": "My amazing extension"
    }

author
^^^^^^
Компания, разработчик, учреждение и т.д., Которые разработали расширение:

.. code-block:: json

    {
        "author": "Tony Hawk"
    }

version
^^^^^^^
Версия расширения должна следовать регулярному выражению: [0-9]+\.[0-9]+\.[0-9]+:

.. code-block:: json

    {
        "version": "1.2.0"
    }

warnings
^^^^^^^^
Предупреждения компилятора включены или отключены в текущем проекте:

.. code-block:: json

    {
        "warnings": {
            "unused-variable": true,
            "unused-variable-external": false,
            "possible-wrong-parameter": true,
            "possible-wrong-parameter-undefined": false,
            "nonexistent-function": true,
            "nonexistent-class": true
        }
    }

optimizations
^^^^^^^^^^^^^
Оптимизация компилятора включена или отключена в текущем проекте:

.. code-block:: json

    {
        "optimizations": {
            "static-type-inference": true,
            "static-type-inference-second-pass": true,
            "local-context-pass": false
        }
    }

globals
^^^^^^^
Доступные для расширения переменные globals. Для получения дополнительной информации см. Главу :doc:`extension globals <globals>`.

.. code-block:: json

    {
        "globals": {
            "my_setting_1": {
                "type": "bool",
                "default": true
            },
            "my_setting_2": {
                "type": "int",
                "default": 10
            }
        }
    }

info
^^^^
phpinfo() Для получения дополнительной информации см. Главу :doc:`phpinfo() <phpinfo>`.

.. code-block:: json

    {
        "info": [
            {
                "header": ["Directive", "Value"],
                "rows": [
                    ["setting1", "value1"],
                    ["setting2", "value2"]
                ]
            }
        ]
    }

extra-cflags
^^^^^^^^^^^^
Любые дополнительные флаги, которые вы хотите добавить в процесс компиляции:

.. code-block:: json

    {
        "extra-cflags": "-I/usr/local/Cellar/libevent/2.0.21_1/include"
    }

extra-libs
^^^^^^^^^^
Любые дополнительные библиотеки, которые вы хотите добавить в процесс компиляции:

.. code-block:: json

    {
        "extra-libs": "-L/usr/local/Cellar/libevent/2.0.21_1/lib -levent"
    }

package-dependencies
^^^^^^^^^^^^^^^^^^^^
Объявление библиотечных зависимостей (проверка версии по :code:`pkg-config`)

.. code-block:: json

    {
        "package-dependencies": {
            "openssl": "*",
            "libpng": ">= 0.1.0",
            "protobuf": "<= 2.6.1"
        }
    }

Оператор версии поддерживает :code:`=`, :code:`>=`, :code:`<=`, and :code:`*`
