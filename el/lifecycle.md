# Lifecycle hooks

PHP provides several lifecycle events, which extensions can use to perform common initialization or shutdown tasks. Normally,Zephir's own hooks into these events will cover all the setup and tear down your extension will need, but if you find that you need to do something more, there are a few options you can use to pass your own code into these same hooks.

Consider the following diagram:

![](/images/content/lifecycle.png)

Lifecycle hooks are registered in the `config.json` file. As you can see in the diagram above, there are four types of lifecycle hooks - `globals`, `initializers`, `destructors`, and `info`. Each of these has its own corresponding root-level setting in the configuration, and both :doc:'globals <globals>' and :doc:'info <phpinfo>' have their own chapters. This chapter covers the other two settings.

You can register an `include` and a `code` for each group's supported `INIT` and `SHUTDOWN` events. The `code` can be whatever you need/want, but a single function call per hook is recommended, both for clarity in the config, and to keep code in other files as much as possible. You can safely omit either the `include` or `code` option, but duplicate `include` options are removed, so you can safely repeat those, instead. It is recommended to provide both values, to make it easier to see which includes are needed for which hooks, and make it easier to add and remove hooks individually.

<a name='initializers'></a>

## initializers

The `initializers` block looks something like this:

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

<a name='desctructors'></a>

## destructors

And the `destructors` block like this:

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