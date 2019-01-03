---
layout: default
language: 'zh-CN'
version: '0.11'
menu:
  - text:
      'initializers'
    url: '#initializers'
  - text:
      'destructors'
    url: '#destructors'
---
# 生命周期钩子

PHP提供了几个生命周期事件，这些扩展可以用来执行常见的初始化或关闭任务。 通常，Zephir在这些事件中自己的钩子会覆盖所有设置，并删除您需要的扩展，但是如果您发现需要做更多的事情，可以使用一些选项将您自己的代码传递到这些相同的钩子中。

考虑下面的图表:

![PHP进程/请求生命周期](/assets/content/lifecycle.png)

生命周期钩子注册在`config.json`。 如上图所示，有四种生命周期钩子 — `globals`， `initializers`，`destructors`，`info`。 每一个都在配置中有自己对应的根级别设置，[globals](/{{ page.version }}/{{ page.language }}/globals)和[info](/{{ page.version }}/{{ page.language }}/phpinfo)都有自己的章节。 本章将介绍另外两种设置。

每个钩子在`config.json`文件是一个对象数组，其本身本质上是`include`/`code`对。 `include`值，如果还没有，则会拉入一个给定的C头文件，这样`code`就可以访问它的内容。 `code`值是由钩的逻辑本身, 虽然在技术上你可以把任何有效的C, 它是*** 强烈***建议把逻辑超过一个或两个行到一个单独的C源文件(比如一个拉连同你的`include` d头文件), 并使用一个单行的函数调用。

<a name='initializers'></a>

## initializers

`initializers` 块如下所示:

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
    

这个块负责定义到上面图中显示的Init事件的钩子。 其中有三个:`globals`用于设置全局变量空间;`module`用于设置扩展本身需要功能的任何内容;`request`用于设置扩展来处理单个请求。

<a name='desctructors'></a>

## destructors

`destructors` 块如下所示:

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

正如 `initializers` 块负责定义上图中显示的 init 事件中的挂钩一样, *this* 块负责定义关机事件中的挂钩。 有四个:`request`响应之前敲定任何数据发送到客户端,`post-request`清理响应被发送后,`module</0 >模块扩展后的清理本身PHP进程关闭之前, 和<code>globals`清理全局变量空间。
