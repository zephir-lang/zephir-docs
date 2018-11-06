# 生命周期钩子

PHP提供了几个生命周期事件，这些扩展可以用来执行常见的初始化或关闭任务。 通常，Zephir在这些事件中自己的钩子会覆盖所有设置，并删除您需要的扩展，但是如果您发现需要做更多的事情，可以使用一些选项将您自己的代码传递到这些相同的钩子中。

考虑下面的图表:

![](/images/content/lifecycle.png)

生命周期钩子注册在`config.json< / 0 >。 如上图所示，有四种生命周期钩子 — <code>globals`， `initializers`，`destructors`，`info`。 每一个都在配置中有自己对应的根级别设置，[globals](/[[language]]/[[version]]/globals)和[info](/[[language]]/[[version]]/phpinfo)都有自己的章节。 本章将介绍另外两种设置。

您可以为每个支持`INIT`和`SHUTDOWN`的组事件注册一个`include`和`code`。 `code` 可以是您需要的任何东西, 但建议每个钩子调用一个函数, 这既是为了在配置中清晰, 也是为了尽可能地将代码保留在其他文件中。 You can safely omit either the `include` or `code` option, but duplicate `include` options are removed, so you can safely repeat those, instead. 建议同时提供这两个值, 以便更轻松地查看哪些包含需要哪些挂钩, 并使单独添加和删除挂钩变得更容易。

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
    

<a name='desctructors'></a>

## destructors

而 `destructors` 块, 如下所示:

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