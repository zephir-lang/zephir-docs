# 配置文件

每个Zephir扩展都有一个名为`config.json`的配置文件。 每当构建或生成扩展时，Zephir都会读取这个文件，它允许开发人员修改扩展或编译器的行为。

该文件使用[JSON](http://en.wikipedia.org/wiki/JSON)作为配置格式:

    {
        "namespace": "test",
        "name": "Test Extension",
        "description": "My amazing extension",
        "author": "Tony Hawk",
        "version": "1.2.0"
    }
    

此文件中定义的设置覆盖Zephir提供的任何默认设置。

支持以下设置:

<a name='api'></a>

## api

用于为您的扩展配置自动生成的HTML文档。 `path`指定在何处创建与项目根相关的文档。 使用`base-url`生成`sitemap.xml`文件为您的文档。 `theme`用于设置用于生成文档的主题(通过`name`设置)，以及主题支持传递的任何选项(通过`options`设置)。 最后，`theme-directories`被用来提供额外的搜索路径，以找到你想要的主题。

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
    

<a name='author'></a>

## 作者

开发扩展的公司、开发商、机构等:

```json
{
    "author": "Tony Hawk"
}
```

<a name='backend'></a>

## backend

Provides a way to configure the Zend Engine backend used by your extension. At the moment, only the `templatepath`, which lets you select between `ZendEngine2` and `ZendEngine3`, is supported:

    {
        "backend": {
            "templatepath": "ZendEngine3"
        }
    }
    

<a name='constants-sources'></a>

## constants-sources

To import just the constants in a C source file into your project, list the file's path in this setting:

    {
        "constants-sources": [
            "utils/math_constants.h"
        ]
    }
    

<a name='description'></a>

## description

Extension description - any text describing your extension:

    {
        "description": "My amazing extension"
    }
    

<a name='destructors'></a>

## destructors

This setting lets you provide one or more C functions to be executed on certain extension lifecycle events - specifically, `RSHUTDOWN` (`request`), `PRSHUTDOWN` (`post-request`), `MSHUTDOWN` (`module`), and `GSHUTDOWN` (`globals`). Check the [lifecycle hooks](/[[language]]/[[version]]/lifecycle) chapter for more information.

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
    

<a name='extension-name'></a>

## extension-name

The base filename of the extension. It must follow the same rules as the `namespace` setting, which is used as a fallback in case this one isn't given.

    {
        "extension-name": "test"
    }
    

<a name='external-dependencies'></a>

## external-dependencies

You can include a class from another namespace/extension directly in your own extension by configuring it here:

    {
        "external-dependencies": {
            "My\\Awesome": "my/awesome/class.zep",
            "My\\Awful": "my/awful/class.zep"
        }
    }
    

<a name='extra'></a>

## extra

Contains extra settings that also can be passed, as is, on the command line. Currently, that's `export-clases` (generate headers for accessing your classes from other C code), and `indent` (select between using `tabs` or `spaces` to indent code in generated files):

    {
        "extra": {
            "export-classes": true,
            "indent": "tabs"
        }
    }
    

<a name='extra-cflags'></a>

## extra-cflags

Any additional flags you want to add to the compilation process:

    {
        "extra-cflags": "-I/usr/local/Cellar/libevent/2.0.21_1/include"
    }
    

<a name='extra-classes'></a>

## extra-classes

If you already have a PHP class implemented in C, you can include it directly in your extension by configuring it here:

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
    

<a name='extra-libs'></a>

## extra-libs

Any additional libraries you want to add to the compilation process:

    {
        "extra-libs": "-L/usr/local/Cellar/libevent/2.0.21_1/lib -levent"
    }
    

<a name='extra-sources'></a>

## extra-sources

Any additional files you want to add to the compilation process - the search directory is relative to the `ext` folder of your project:

    {
        "extra-sources": [
            "utils/pi.c"
        ]
    }
    

<a name='globals'></a>

## globals

Extension globals available. Check the [globals](/[[language]]/[[version]]/globals) chapter for more information.

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
    

<a name='info'></a>

## info

`phpinfo()` sections. Check the [phpinfo()](/[[language]]/[[version]]/phpinfo) chapter for more information.

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
    

<a name='initializers'></a>

## initializers

This setting lets you provide one or more C functions to be executed on certain extension lifecycle events - specifically, `GINIT` (`globals`), `MINIT` (`module`), and `RINIT` (`request`). Check the [lifecycle hooks](/[[language]]/[[version]]/lifecycle) chapter for more information.

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
    

<a name='name'></a>

## name

Extension name used in compiled C code - can only contain ascii characters:

    {
        "name": "test"
    }
    

<a name='namespace'></a>

## namespace

The namespace of the extension - it must be a simple identifier respecting the regular expression `[a-zA-Z0-9\_]+`:

    {
        "namespace": "test"
    }
    

<a name='optimizations'></a>

## optimizations

Compiler optimizations which should be enabled or disabled in the current project:

    {
        "optimizations": {
            "static-type-inference": true,
            "static-type-inference-second-pass": true,
            "local-context-pass": false
        }
    }
    

<a name='optimizer-dirs'></a>

## optimizer-dirs

The directories where your own optimizers can be found - the search directory is relative to the root folder of your project:

    {
        "optimizer-dirs": [
            "optimizers"
        ]
    }
    

<a name='package-dependencies'></a>

## package-dependencies

Declare library dependencies (version constraints will be checked by `pkg-config`, and can use one of the operators `=`, `>=`, `<=`, or `*`):

    {
        "package-dependencies": {
            "openssl": "*",
            "libpng": ">= 0.1.0",
            "protobuf": "<= 2.6.1"
        }
    }
    

<a name='prototype-dir'></a>

## prototype-dir

Allows you to provide prototype files describing other extensions required to build your own, so they don't necessarily need to be installed during the build phase:

    {
        "prototype-dir": {
            "igbinary": "prototypes",
            "session": "prototypes"
        }
    }
    

<a name='requires'></a>

## requires

Allows you to list other extensions as required to build/use your own:

    {
        "requires": {
            "extensions": [
                "igbinary",
                "session"
            ]
        }
    }
    

<a name='silent'></a>

## silent

Suppresses most/all output from `zephir` commands (same as `-w`):

    {
        "silent": false
    }
    

<a name='stubs'></a>

## stubs

This setting allows adjusting the way IDE documentation stubs are generated. `path` sets where the stubs should be created, while `stubs-run-after-generate` sets whether to automatically (re)build the stubs when your code is compiled to C:

    {
        "stubs": {
            "path": "ide/%version%/%namespace%/",
            "stubs-run-after-generate": false
        }
    }
    

<a name='verbose'></a>

## verbose

Displays more detail in error messages from exceptions generated by `zephir` commands (can also enable with `-v`, or disable with `-V`):

    {
        "verbose": false
    }
    

<a name='version'></a>

## version

Extension version - must follow the regular expression `[0-9]+\.[0-9]+\.[0-9]+`:

    {
        "version": "1.2.0"
    }
    

<a name='warnings'></a>

## warnings

Compiler warnings which should be enabled or disabled in the current project:

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