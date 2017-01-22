Phpinfo() 部分
==================
像大多数扩展一样，Zephir扩展也可以在phpinfo()中显示一些信息。这些信息通常与指令，环境数据等相关。

默认情况下，每个Zephir扩展会自动的向phpinfo()的输出的表格中添加一些数据以显示扩展的版本号等信息。

我们可以添加如下的指令到config.json文件中：

.. code-block:: javascript

    "info": [
        {
            "header": ["Directive", "Value"],
            "rows": [
                ["setting1", "value1"],
                ["setting2", "value2"]
            ]
        },
        {
            "header": ["Directive", "Value"],
            "rows": [
                ["setting3", "value3"],
                ["setting4", "value4"]
            ]
        }
    ]


如下显示：

.. figure:: ../_static/img/info.png
    :align: center
