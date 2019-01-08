---
layout: default
language: 'zh-cn'
version: '0.10'
---
# 配置文件

每个Zephir扩展都有一个名为`config.json`的配置文件。 每当构建或生成扩展时，Zephir都会读取这个文件，它允许开发人员修改扩展或编译器的行为。

该文件使用[JSON](http://en.wikipedia.org/wiki/JSON)作为配置格式:

```json
{
    "namespace": "test",
    "name": "Test Extension",
    "description": "My amazing extension",
    "author": "Tony Hawk",
    "version": "1.2.0"
}
```

此文件中定义的设置覆盖Zephir提供的任何默认设置。

支持以下设置:

<a name='api'></a>

## api

用于为您的扩展配置自动生成的HTML文档。 `path`指定在何处创建与项目根相关的文档。 使用`base-url`生成`sitemap.xml`文件为您的文档。 `theme`用于设置用于生成文档的主题(通过`name`设置)，以及主题支持传递的任何选项(通过`options`设置)。 最后，`theme-directories`被用来提供额外的搜索路径，以找到你想要的主题。

```json
{
    "api": {
        "path": "doc/%version%",
        "base-url": "http://example.local/api/",
        "theme": {
            "name"   : "zephir",
            "options": {
                "github":           null,
                "analytics":        null,
                "main_color":       "#3E6496",
                "link_color":       "#3E6496",
                "link_hover_color": "#5F9AE7"
            }
        },
        "theme-directories": [
            "my/api/themes"
        ]
    }
}
```

<a name='author'></a>

## author

开发扩展的公司、开发商、机构等:

```json
{
    "author": "Tony Hawk"
}
```

<a name='backend'></a>

## backend

提供一种配置扩展所使用的Zend引擎后端的方法。 目前，仅支持`templatepath`，允许您在`ZendEngine2` </code> ZendEngine3</0>之间进行选择:

```json
{
    "backend": {
        "templatepath": "ZendEngine3"
    }
}
```

<a name='constants-sources'></a>

## constants-sources

要将C源文件中的常量导入到项目中，请在此设置中列出文件的路径:

```json
{
    "constants-sources": [
        "utils/math_constants.h"
    ]
}
```

<a name='description'></a>

## description

扩展描述-任何文字描述您的扩展:

```json
{
    "description": "My amazing extension"
}
```

<a name='destructors'></a>

## destructors

此设置允许您提供一个或多个C函数在某些扩展生命周期事件上执行——具体来说，`RSHUTDOWN`(`请求`)，`PRSHUTDOWN` (`post请求`)，`MSHUTDOWN` (<0 >0 module</0 >1)， <0 >2 GSHUTDOWN</0 >3 (<0 >4 globals</0 >5)。 Check the [lifecycle hooks](/0.11/zh-cn/lifecycle) chapter for more information.

```json
{
    "destructors": [
        {
            "request": [
                {
                    "include": "my/awesome/library.h",
                    "code": "c_function_for_shutting_down(TSRMLS_C)"
                },
                {
                    "include": "my/awful/library.h",
                    "code": "some_other_c_function_than_the_other_ones(TSRMLS_C)"
                }
            ],
            "post-request": [
                {
                    "include": "my/awesome/library.h",
                    "code": "c_function_for_cleaning_up_after_the_response_is_sent(TSRMLS_C)"
                }
            ],
            "module": [
                {
                    "include": "my/awesome/library.h",
                    "code": "release_module_deps(TSRMLS_C)"
                }
            ],
            "globals": [
                {
                    "include": "my/awesome/library.h",
                    "code": "release_globals_deps(TSRMLS_C)"
                }
            ]
        }
    ]
}
```

<a name='extension-name'></a>

## extension-name

扩展的基本文件名。 它必须遵循与`namespace`设置相同的规则，如果没有给出>设置，则将其用作后备。

```json
{
    "extension-name": "test"
}
```

<a name='external-dependencies'></a>

## external-dependencies

您可以在自己的扩展中直接包含来自另一个名称空间/扩展的类，在这里进行配置:

```json
{
    "external-dependencies": {
        "My\\Awesome": "my/awesome/class.zep",
        "My\\Awful": "my/awful/class.zep"
    }
}
```

<a name='extra'></a>

## extra

包含额外的设置, 这些设置也可以像在命令行中一样传递。 目前，这是`export-clases`(生成从其他C代码访问类的头文件)，和`indent`(选择使用`tabs`或`spaces`缩进生成的文件中的代码):

```json
{
    "extra": {
        "export-classes": true,
        "indent": "tabs"
    }
}
```

<a name='extra-cflags'></a>

## extra-cflags

您想要添加到编译过程中的任何附加标志:

```json
{
    "extra-cflags": "-I/usr/local/Cellar/libevent/2.0.21_1/include"
}
```

<a name='extra-classes'></a>

## extra-classes

如果你已经在C语言中实现了一个PHP类，你可以直接将它包含在你的扩展中，在这里进行配置:

```json
{
    "extra-classes": [
        {
            "header": "utls/old_c_class/class.h",
            "source": "utils/old_c_class/class.c",
            "init": "old_c_class",
            "entry": "old_c_class_ce"
        }
    ]
}
```

<a name='extra-libs'></a>

## extra-libs

您想要添加到编译过程中的任何其他库:

```json
{
    "extra-libs": "-L/usr/local/Cellar/libevent/2.0.21_1/lib -levent"
}
```

<a name='extra-sources'></a>

## extra-sources

任何其他文件，你想添加到编译过程-搜索目录是相对于`ext`文件夹您的项目:

```json
{
    "extra-sources": [
        "utils/pi.c"
    ]
}
```

<a name='globals'></a>

## globals

扩展全局可用。 Check the [globals](/0.11/zh-cn/globals) chapter for more information.

```json
{
    "globals": {
        "my_setting_1": {
            "type": "bool",
            "default": true
        },
        "my_setting_2": {
            "type": "int",
            "default": 10
        }
    }
}
```

<a name='info'></a>

## info

`phpinfo()` 信息. Check the [phpinfo()](/0.11/zh-cn/phpinfo) chapter for more information.

```json
{
    "info": [
        {
            "header": ["Directive", "Value"],
            "rows": [
                ["setting1", "value1"],
                ["setting2", "value2"]
            ]
        }
    ]
}
```

<a name='initializers'></a>

## initializers

这个设置允许您提供一个或多个C函数在某些扩展生命周期事件上执行——具体来说，`GINIT` (`globals`)， `MINIT` (`module`)， `RINIT` (<0 >0 request</0 >1)。 Check the [lifecycle hooks](/0.11/zh-cn/lifecycle) chapter for more information.

```json
{
    "initializers": [
        {
            "globals": [
                {
                    "include": "my/awesome/library.h",
                    "code": "setup_globals_deps(TSRMLS_C)"
                }
            ],
            "module": [
                {
                    "include": "my/awesome/library.h",
                    "code": "setup_module_deps(TSRMLS_C)"
                }
            ],
            "request": [
                {
                    "include": "my/awesome/library.h",
                    "code": "some_c_function(TSRMLS_C)"
                },
                {
                    "include": "my/awful/library.h",
                    "code": "some_other_c_function(TSRMLS_C)"
                }
            ]
        }
    ]
}
```

<a name='name'></a>

## name

在编译后的C代码中使用的扩展名-只能包含ascii字符:

```json
{
    "name": "test"
}
```

<a name='namespace'></a>

## namespace

扩展的名称空间-它必须是一个简单的标识符，对应于正则表达式`[a- za - z0 -9\_]+`:

```json
{
    "namespace": "test"
}
```

<a name='optimizations'></a>

## optimizations

在当前项目中应该启用或禁用的编译器优化:

```json
{
    "optimizations": {
        "static-type-inference": true,
        "static-type-inference-second-pass": true,
        "local-context-pass": false
    }
}
```

<a name='optimizer-dirs'></a>

## optimizer-dirs

你自己的优化器可以找到的目录-搜索目录是相对于根文件夹的项目:

```json
{
    "optimizer-dirs": [
        "optimizers"
    ]
}
```

<a name='package-dependencies'></a>

## package-dependencies

声明库依赖关系(版本约束将被`pkg-config`检查，可以使用`=`，`>=`， `<=`，或`*`):

```json
{
    "package-dependencies": {
        "openssl": "*",
        "libpng": ">= 0.1.0",
        "protobuf": "<= 2.6.1"
    }
}
```

<a name='prototype-dir'></a>

## prototype-dir

允许您提供描述构建自己的扩展所需的其他扩展的原型文件，因此它们不需要在构建阶段安装:

```json
{
    "prototype-dir": {
        "igbinary": "prototypes",
        "session": "prototypes"
    }
}
```

<a name='requires'></a>

## requires

允许您列出其他扩展所需的建立/使用您自己:

```json
{
    "requires": {
        "extensions": [
            "igbinary",
            "session"
        ]
    }
}
```

<a name='silent'></a>

## silent

允许您列出其他扩展所需的建立/使用您自己:

```json
{
    "silent": false
}
```

<a name='stubs'></a>

## stubs

此设置允许调整IDE文档存根生成的方式。 `path`集，其中应该创建存根，而`stubs-run-after-generate`集，当您的代码被编译为C时，是否自动(重新)构建存根:

```json
{
    "stubs": {
        "path": "ide/%version%/%namespace%/",
        "stubs-run-after-generate": false
    }
}
```

<a name='verbose'></a>

## verbose

在错误消息中显示由`zephir`命令生成的异常的更多细节(也可以启用`-v`，或禁用`-V`):

```json
{
    "verbose": false
}
```

<a name='version'></a>

## version

扩展版本-必须遵循正则表达式`[0-9]+\.[0-9]+\.[0-9]+`:

```json
{
    "version": "1.2.0"
}
```

<a name='warnings'></a>

## warnings

在当前项目中应该启用或禁用的编译器警告:

```json
{
    "warnings": {
        "unused-variable": true,
        "unused-variable-external": false,
        "possible-wrong-parameter": true,
        "possible-wrong-parameter-undefined": false,
        "nonexistent-function": true,
        "nonexistent-class": true
    }
}
```
