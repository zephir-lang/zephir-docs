---
layout: default
language: 'ru-ru'
version: '0.11'
menu:
  - text:
      'api'
    url: '#api'
  - text:
      'author'
    url: '#author'
  - text:
      'backend'
    url: '#backend'
  - text:
      'constants-sources'
    url: '#constants-sources'
  - text:
      'description'
    url: '#description'
  - text:
      'destructors'
    url: '#destructors'
  - text:
      'extension-name'
    url: '#extension-name'
  - text:
      'external-dependencies'
    url: '#external-dependencies'
  - text:
      'extra'
    url: '#extra'
  - text:
      'extra-cflags'
    url: '#extra-cflags'
  - text:
      'extra-classes'
    url: '#extra-classes'
  - text:
      'extra-libs'
    url: '#extra-libs'
  - text:
      'extra-sources'
    url: '#extra-sources'
  - text:
      'globals'
    url: '#globals'
  - text:
      'info'
    url: '#info'
  - text:
      'initializers'
    url: '#initializers'
  - text:
      'name'
    url: '#name'
  - text:
      'namespace'
    url: '#namespace'
  - text:
      'optimizations'
    url: '#optimizations'
  - text:
      'optimizer-dirs'    
    url: '#optimizer-dirs'
  - text:
      'package-dependencies'
    url: '#package-dependencies'
  - text:
      'prototype-dir'
    url: '#prototype-dir'
  - text:
      'requires'
    url: '#requires'
  - text:
      'silent'
    url: '#silent'
  - text:
      'stubs'
    url: '#stubs'
  - text:
      'verbose'
    url: '#verbose'
  - text:
      'version'
    url: '#version'
  - text:
      'warnings'
    url: '#warnings'
---
# Configuration File

Every Zephir extension has a configuration file called `config.json`. This file is read by Zephir every time you build or generate the extension, and it allows the developer to modify the extension's or compiler's behavior.

This file uses [JSON](http://en.wikipedia.org/wiki/JSON) as its configuration format:

```json
{
    "namespace": "test",
    "name": "Test Extension",
    "description": "My amazing extension",
    "author": "Tony Hawk",
    "version": "1.2.0"
}
```

Settings defined in this file override any factory default setting provided by Zephir.

The following settings are supported:

<a name='api'></a>

## api

Used to configure the automatically generated HTML documentation for your extension. `path` specifies where to create the documentation relative to the project root. `base-url` is used to generate a `sitemap.xml` file for your documentation. `theme` is used to set the theme used for the generated documentation (via the `name` setting), and any options the theme supports passing (via the `options` setting). Finally, `theme-directories` is used to provide additional search paths for finding your desired theme.:

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

Company, developer, institution, etc that developed the extension:

```json
{
    "author": "Tony Hawk"
}
```

<a name='backend'></a>

## backend

Provides a way to configure the Zend Engine backend used by your extension. At the moment, only the `templatepath`, which lets you select between `ZendEngine2` and `ZendEngine3`, is supported:

```json
{
    "backend": {
        "templatepath": "ZendEngine3"
    }
}
```

<a name='constants-sources'></a>

## constants-sources

To import just the constants in a C source file into your project, list the file's path in this setting:

```json
{
    "constants-sources": [
        "utils/math_constants.h"
    ]
}
```

<a name='description'></a>

## description

Extension description - any text describing your extension:

```json
{
    "description": "My amazing extension"
}
```

<a name='destructors'></a>

## destructors

This setting lets you provide one or more C functions to be executed on certain extension lifecycle events - specifically, `RSHUTDOWN` (`request`), `PRSHUTDOWN` (`post-request`), `MSHUTDOWN` (`module`), and `GSHUTDOWN` (`globals`). Check the [lifecycle hooks](/{{ page.version }}/{{ page.language }}/lifecycle) chapter for more information.

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

The base filename of the extension. It must follow the same rules as the `namespace` setting, which is used as a fallback in case this one isn't given.

```json
{
    "extension-name": "test"
}
```

<a name='external-dependencies'></a>

## external-dependencies

You can include a class from another namespace/extension directly in your own extension by configuring it here:

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

Contains extra settings that also can be passed, as is, on the command line. Currently, that's `export-clases` (generate headers for accessing your classes from other C code), and `indent` (select between using `tabs` or `spaces` to indent code in generated files):

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

Any additional flags you want to add to the compilation process:

```json
{
    "extra-cflags": "-I/usr/local/Cellar/libevent/2.0.21_1/include"
}
```

<a name='extra-classes'></a>

## extra-classes

If you already have a PHP class implemented in C, you can include it directly in your extension by configuring it here:

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

Any additional libraries you want to add to the compilation process:

```json
{
    "extra-libs": "-L/usr/local/Cellar/libevent/2.0.21_1/lib -levent"
}
```

<a name='extra-sources'></a>

## extra-sources

Any additional files you want to add to the compilation process - the search directory is relative to the `ext` folder of your project:

```json
{
    "extra-sources": [
        "utils/pi.c"
    ]
}
```

<a name='globals'></a>

## globals

Extension globals available. Check the [globals](/{{ page.version }}/{{ page.language }}/globals) chapter for more information.

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

`phpinfo()` sections. Check the [phpinfo()](/{{ page.version }}/{{ page.language }}/phpinfo) chapter for more information.

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

This setting lets you provide one or more C functions to be executed on certain extension lifecycle events - specifically, `GINIT` (`globals`), `MINIT` (`module`), and `RINIT` (`request`). Check the [lifecycle hooks](/{{ page.version }}/{{ page.language }}/lifecycle) chapter for more information.

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

Extension name used in compiled C code - can only contain ascii characters:

```json
{
    "name": "test"
}
```

<a name='namespace'></a>

## namespace

The namespace of the extension - it must be a simple identifier respecting the regular expression `[a-zA-Z0-9\_]+`:

```json
{
    "namespace": "test"
}
```

<a name='optimizations'></a>

## optimizations

Compiler optimizations which should be enabled or disabled in the current project:

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

The directories where your own optimizers can be found - the search directory is relative to the root folder of your project:

```json
{
    "optimizer-dirs": [
        "optimizers"
    ]
}
```

<a name='package-dependencies'></a>

## package-dependencies

Declare library dependencies (version constraints will be checked by `pkg-config`, and can use one of the operators `=`, `>=`, `<=`, or `*`):

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

Allows you to provide prototype files describing other extensions required to build your own, so they don't necessarily need to be installed during the build phase:

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

Allows you to list other extensions as required to build/use your own:

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

Suppresses most/all output from `zephir` commands (same as `-w`):

```json
{
    "silent": false
}
```

<a name='stubs'></a>

## stubs

This setting allows adjusting the way IDE documentation stubs are generated. `path` sets where the stubs should be created, while `stubs-run-after-generate` sets whether to automatically (re)build the stubs when your code is compiled to C:

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

Displays more detail in error messages from exceptions generated by `zephir` commands (can also enable with `-v`, or disable with `-V`):

```json
{
    "verbose": false
}
```

<a name='version'></a>

## version

Extension version - must follow the regular expression `[0-9]+\.[0-9]+\.[0-9]+`:

```json
{
    "version": "1.2.0"
}
```

<a name='warnings'></a>

## warnings

Compiler warnings which should be enabled or disabled in the current project:

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
