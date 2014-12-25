Phpinfo() sections
==================
Like most extensions, Zephir extensions are able to show information at the phpinfo() output.
These information is usually related to directives, enviroment data, etc.

By default, every Zephir extension automatically adds a basic table to the phpinfo() output
showing the extension version.

You can add more directives by adding the following configuration to the config.json file:

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

These information will be shown as follows:

.. figure:: ../_static/img/info.png
    :align: center
