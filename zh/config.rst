配置文件
==================
每一个Zephir扩展都有一个配置文件叫做 config.json。 Zephir在每次建立或生成扩展时将读取该文件。
他允许开发人员修改扩展或编译器的行为。

该文件使用 `JSON <http://en.wikipedia.org/wiki/JSON>`_ 配置格式是非常简单和友好的:

.. code-block:: json

    {
        "namespace": "test",
        "name": "Test Extension",
        "description": "My amazing extension",
        "author": "Tony Hawk",
        "version": "1.2.0"
    }

这个文件里定义的配置项会覆盖掉Zephir的默认配置项.

Zephir支持如下的配置项:

命名空间
^^^^^^^^^
扩展的命名空间, 必须遵循正则表达式: [a-zA-Z0-9\_]+:

.. code-block:: json

    {
        "namespace": "test"
    }

名称
^^^^
扩展的名称，只能包含ASCII字符:

.. code-block:: json

    {
        "namespace": "test"
    }

描述
^^^^^^^^^^^
扩展的描述, 描述扩展的功能等:

.. code-block:: json

    {
        "description": "My amazing extension"
    }

作者
^^^^^^
开发扩展的公司, 开发者, 机构等:

.. code-block:: json

    {
        "author": "Tony Hawk"
    }

版本
^^^^^^^
扩展的版本, 必须遵循正则表达式: [0-9]+\.[0-9]+\.[0-9]+:

.. code-block:: json

    {
        "version": "1.2.0"
    }

警告
^^^^^^^^
当前项目启用或禁用编译器警告:

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

优化
^^^^^^^^^^^^^
当前的扩展在编译时打开或关闭的优化选项:

.. code-block:: json

    {
        "optimizations": {
            "static-type-inference": true,
            "static-type-inference-second-pass": true,
            "local-context-pass": false
        }
    }

全局变量
^^^^^^^
扩展的全局变量。可以参见 :doc:`extension globals <globals>` 章节以获取更多信息.

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

信息
^^^^
phpinfo() 部分 检查 :doc:`phpinfo() <phpinfo>` 更多的信息。

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
  
附加的C编译选项
^^^^^^^^^^^^
编译过程中可选的编译选项写在这里:

.. code-block:: json

    {
        "extra-cflags": "-I/usr/local/Cellar/libevent/2.0.21_1/include"
    }

附加的c库
^^^^^^^^^^
编译过程中需要的库可以写在这里:

.. code-block:: json

    {
        "extra-libs": "-L/usr/local/Cellar/libevent/2.0.21_1/lib -levent"
    }

附加资源
^^^^^^^^^^^^^
编译过程中需要的附加文件文件写在这里:

.. code-block:: json

    {
        "extra-sources": ["utils/pi.c"]
    }
搜索路径相对于扩展的ext文件夹

优化器目录
^^^^^^^^^^^^^^
这里是优化器所在的目录:

.. code-block:: json

    {
        "optimizer-dirs": ["optimizer-dirs"]
    }
搜索路径相对于项目的根目录

包依赖
^^^^^^^^^^^^^^^^^^^^
声明依赖的库(version check by :code:`pkg-config`)

.. code-block:: json

    {
        "package-dependencies": {
            "openssl": "*",
            "libpng": ">= 0.1.0",
            "protobuf": "<= 2.6.1"
        }
    }

版本对比支持的操作如右 :code:`=`, :code:`>=`, :code:`<=`, and :code:`*`
    