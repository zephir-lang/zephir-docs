---
layout: default
language: 'el-GR'
version: '0.11'
---
# Phpinfo() sections

Like most extensions, Zephir extensions are able to show information in the [phpinfo()](http://php.net/manual/en/function.phpinfo.php) output. This information is usually related to directives, environment data, etc.

By default, every Zephir extension automatically adds a basic table to the `phpinfo()` output showing the extension version, and any INI options the extension supports.

You can add more directives by adding the following configuration to the `config.json` file:

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
    

This information will be shown as follows:

![](/assets/content/info.png)