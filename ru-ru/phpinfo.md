---
layout: default
language: 'en'
version: '0.12'
---

# Секции phpinfo()

Как и большинство расширений, Zephir расширения могут отображать информацию при выводе [phpinfo()](http://php.net/manual/en/function.phpinfo.php). Обычно эта информация относится к директивам, данным окружения и т.п.

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