# 生命周期钩子

PHP提供了几个生命周期事件，这些扩展可以用来执行常见的初始化或关闭任务。 Normally, Zephir's own hooks into these events will cover all the setup and tear down your extension will need, but if you find that you need to do something more, there are a few options you can use to pass your own code into these same hooks.

考虑下面的图表:

![The PHP Process/Request Lifecycle](/images/content/lifecycle.png)

生命周期钩子注册在`config.json`。 如上图所示，有四种生命周期钩子 — `globals`， `initializers`，`destructors`，`info`。 每一个都在配置中有自己对应的根级别设置，[globals](/[[language]]/[[version]]/globals)和[info](/[[language]]/[[version]]/phpinfo)都有自己的章节。 本章将介绍另外两种设置。

Each hook in the `config.json` file is an array of objects, which themselves are essentially `include`/`code` pairs. The `include` value will pull in a given C header file, if it hasn't been already, so that the `code` will have access to its contents. The `code` value is the logic run by the hook itself, and while you can technically put any valid C in here, it is ***strongly*** recommended to put logic longer than one or two lines into a separate C source file (such as the one pulled in along with your `include`d header file), and use a single-line function call here.

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
    

This block is responsible for defining hooks into the Init events shown in the diagram above. There are three of these: `globals` for setting up the global variable space, `module` for setting up anything the extension itself needs to function, and `request` for setting up the extension to handle a single request.

<a name='desctructors'></a>

## destructors

The `destructors` block looks something like this:

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

Much as the `initializers` block is responsible for defining hooks into the Init events shown in the diagram above, *this* block is responsible for defining hooks into the Shutdown events. There are four of these: `request` for finalizing any data before a response is sent to the client, `post-request` for cleaning up after a response has been sent, `module` for cleaning up after the extension itself before the PHP process shuts down, and `globals` for cleaning up the global variable space.