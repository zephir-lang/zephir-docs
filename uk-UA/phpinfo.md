---
layout: default
language: 'uk-UA'
version: '0.10'
---
# Phpinfo() sections

Як і більшість PHP-розширень, Zephir-розширення здатні показати інформацію у виводі [phpinfo()](http://php.net/manual/en/function.phpinfo.php). This information is usually related to directives, environment data, etc.

Типово, кожне Zephir-розширення додає базову таблицю у вивід`phpinfo()`, яка показує версію розширення.

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