# Секции phpinfo()

Like most extensions, Zephir extensions are able to show information in the [phpinfo()](http://php.net/manual/en/function.phpinfo.php) output. Обычно эта информация относится к директивам, данным окружения и т.п.

By default, every Zephir extension automatically adds a basic table to the `phpinfo()` output showing the extension version, and any INI options the extension supports.

Вы можете добавить больше директив, добавив следующую конфигурацию в файл `config.json`:

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
    

Эта информация будет отображена следующим образом:

![](/images/content/info.png)