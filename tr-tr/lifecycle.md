---
layout: default
language: 'tr-tr'
version: '0.12'
---

# Lifecycle hooks

PHP provides several lifecycle events, which extensions can use to perform common initialization or shutdown tasks. Normally, Zephir's own hooks into these events will cover all the setup and tear down your extension will need, but if you find that you need to do something more, there are a few options you can use to pass your own code into these same hooks.

Consider the following diagram:

![The PHP Process/Request Lifecycle](/assets/content/lifecycle.png)

Lifecycle hooks are registered in the `config.json` file. As you can see in the diagram above, there are four types of lifecycle hooks - `globals`, `initializers`, `destructors`, and `info`. Each of these has its own corresponding root-level setting in the configuration, and both [globals](/{{ version }}/{{ language }}/globals) and [info](/{{ version }}/{{ language }}/phpinfo) have their own chapters. This chapter covers the other two settings.

Each hook in the `config.json` file is an array of objects, which themselves are essentially `include`/`code` pairs. The `include` value will pull in a given C header file, if it hasn't been already, so that the `code` will have access to its contents. The `code` value is the logic run by the hook itself, and while you can technically put any valid C in here, it is ***strongly*** recommended to put logic longer than one or two lines into a separate C source file (such as the one pulled in along with your `include`d header file), and use a single-line function call here.

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
