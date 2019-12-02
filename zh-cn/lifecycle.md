---
layout: default
language: 'zh-cn'
version: '0.10'
---

# 生命周期钩子
PHP提供了几个生命周期事件，这些扩展可以用来执行常见的初始化或关闭任务。 通常，Zephir在这些事件中自己的钩子会覆盖所有设置，并删除您需要的扩展，但是如果您发现需要做更多的事情，可以使用一些选项将您自己的代码传递到这些相同的钩子中。

考虑下面的图表:

![PHP进程/请求生命周期](/assets/content/lifecycle.png)

生命周期钩子注册在`config.json`。 如上图所示，有四种生命周期钩子 — `globals`， `initializers`，`destructors`，`info`。 Each of these has its own corresponding root-level setting in the configuration, and both [globals](/0.11/en/globals) and [info](/0.11/en/phpinfo) have their own chapters. 本章将介绍另外两种设置。

每个钩子在`config.json`文件是一个对象数组，其本身本质上是`include`/`code`对。 `include`值，如果还没有，则会拉入一个给定的C头文件，这样`code`就可以访问它的内容。 The `code` value is the logic run by the hook itself, and while you can technically put any valid C in here, it is **_strongly_** recommended to put logic longer than one or two lines into a separate C source file (such as the one pulled in along with your `include`d header file), and use a single-line function call here.

<a name='initializers'></a>

## initializers
`initializers` 块如下所示:

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

Much as the `initializers` block is responsible for defining hooks into the Init events shown in the diagram above, _this_ block is responsible for defining hooks into the Shutdown events. 有四个:`request`响应之前敲定任何数据发送到客户端,`post-request`清理响应被发送后,`module</0 >模块扩展后的清理本身PHP进程关闭之前, 和<code>globals`清理全局变量空间。
