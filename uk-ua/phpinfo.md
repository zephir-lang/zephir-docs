---
layout: default
language: 'uk-ua'
version: '0.11'
---

# Розділи в phpinfo()
Як і більшість PHP-розширень, Zephir-розширення здатні показати інформацію у виводі [phpinfo()](http://php.net/manual/en/function.phpinfo.php). Зазвичай ця інформація відноситься до директив, даних оточення і т.д.

By default, every Zephir extension automatically adds a basic table to the `phpinfo()` output showing the extension version, and any INI options the extension supports.

Ви можете додати більше директив, додавши наступну конфігурацію в файл `config.json`:

```json
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
```

Ця інформація буде показана наступним чином:

![](/assets/content/info.png)
