---
layout: default
language: 'ru-ru'
version: '0.11'
---

# Секции phpinfo()
Like most extensions, Zephir extensions are able to show information in the [phpinfo()](https://php.net/manual/en/function.phpinfo.php) output. Обычно эта информация относится к директивам, данным окружения и т.п.

По умолчанию, каждое Zephir расширение добавляет базовую таблицу в вывод `phpinfo()` отображающую версию расширения.

Вы можете добавить больше директив, добавив следующую конфигурацию в файл `config.json`:

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

Эта информация будет отображена следующим образом:

![](/assets/content/info.png)
