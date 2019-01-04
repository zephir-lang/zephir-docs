---
layout: default
language: 'zh-CN'
version: '0.11'
---
# 全局扩展

PHP扩展提供了一种在扩展中定义全局变量的方法。 读/写全局变量应该比任何其他全局机制（如静态成员）更快。 您可以使用扩展全局变量来设置更改库行为的配置选项。

在Zephir中，扩展全局变量仅限于简单的标量类型，如` int </ 0> / <code> bool </ 0> / <code> double </ 0> / <code> char </ 0>等。 此处不允许使用复杂类型，例如字符串/数组/对象/资源。</p>

<p>您可以通过将以下结构添加到<code> config.json `来启用扩展全局变量：

```json
{
    "globals": {
        "allow_some_feature": {
            "type": "bool",
            "default": true,
            "module": true
        },
        "number_times": {
            "type": "int",
            "default": 10
        },
        "some_component.my_setting_1": {
            "type": "bool",
            "default": true
        },
        "some_component.my_setting_2": {
            "type": "int",
            "default": 100
        }
    }
}
```

每个全局具有以下结构：

```json
"<global-name>": {
    "type": "<some-valid-type>",
    "default": <some-compatible-default-value>
}
```

复合（命名空间）全局变量具有以下结构：

```json
"<namespace>.<global-name>": {
    "type": "<some-valid-type>",
    "default": <some-compatible-default-value>
}
```

可选的`module</ 0>键（如果存在）将全局的初始化过程放入模块范围的<code> GINIT </ 0>生命周期事件中，这意味着它只会在每个PHP进程中设置一次， 而不是为每个请求重新初始化，这是默认值：</p>

<pre><code class="json">{
    "globals": {
        "allow_some_feature": {
            "type": "bool",
            "default": true,
            "module": true
        },
        "number_times": {
            "type": "int",
            "default": 10
        }
    }
}
`</pre> 

在上面的例子中，`allow_some_feature`在启动时只设置一次;`number_times`是在每个请求开始时设置的。

在任何方法中，您可以使用内置函数` globals_get ` / ` globals_set `读/写扩展全局变量：

```zephir
globals_set("allow_some_feature", true);
let someFeature = globals_get("allow_some_feature");
```

如果你想从PHP中改变这些全局变量，一个好的选择是包含一个针对这个的方法:

```zephir
namespace Test;

class MyOptions
{

    public static function setOptions(array options)
    {
        boolean someOption, anotherOption;

        if fetch someOption, options["some_option"] {
            globals_set("some_option", someOption);
        }

        if fetch anotherOption, options["another_option"] {
            globals_set("another_option", anotherOption);
        }
    }
}
```

不能动态访问扩展全局, 因为 `globals_get`/`globals_set` 优化器生成的 c 代码必须在编译时解析:

```zephir
let myOption = "someOption";

// 将抛出编译器异常
let someOption = globals_get(myOption);
```
