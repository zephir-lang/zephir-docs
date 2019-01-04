---
layout: default
language: 'zh-CN'
version: '0.10'
---
# phpinfo () 部分

与大多数扩展一样, Zephir 扩展能够在 [phpinfo()](http://php.net/manual/en/function.phpinfo.php) 输出中显示信息。 这些信息通常与指令、环境数据等有关。

默认情况下, 每个 Zephir扩展都会自动向显示扩展版本和扩展所支持的任何 ini 选项的 `phpinfo()` 输出中添加一个基本表。

通过向 `config.json` 文件中添加以下配置, 可以添加更多指令:

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

此信息将如下所示:

![](/assets/content/info.png)
