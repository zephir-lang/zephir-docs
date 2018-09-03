Configuration File
==================
Every Zephir extension has a configuration file called :code:'config.json'. This file is read by Zephir every time you build
or generate the extension, and it allows the developer to modify the extension's or compiler's behavior.

This file uses 'JSON <http://en.wikipedia.org/wiki/JSON>'_ as its configuration format:

.. code-block:: json

    {
        "namespace": "test",
        "name": "Test Extension",
        "description": "My amazing extension",
        "author": "Tony Hawk",
        "version": "1.2.0"
    }

Settings defined in this file override any factory default setting provided by Zephir.

The following settings are supported:

namespace
^^^^^^^^^
The namespace of the extension - it must be a simple identifier respecting the regular expression :code:'[a-zA-Z0-9\_]+':

.. code-block:: json

    {
        "namespace": "test"
    }

extension-name
^^^^^^^^^^^^^^
The base filename of the extension. It must follow the same rules as the :code:'namespace' setting, which is used as a
fallback in case this one isn't given.

.. code-block:: json

    {
        "extension-name": "test"
    }

name
^^^^
Extension name used in compiled C code - can only contain ascii characters:

.. code-block:: json

    {
        "name": "test"
    }

description
^^^^^^^^^^^
Extension description - any text describing your extension:

.. code-block:: json

    {
        "description": "My amazing extension"
    }

author
^^^^^^
Company, developer, institution, etc that developed the extension:

.. code-block:: json

    {
        "author": "Tony Hawk"
    }

version
^^^^^^^
Extension version - must follow the regular expression :code:'[0-9]+\.[0-9]+\.[0-9]+':

.. code-block:: json

    {
        "version": "1.2.0"
    }

warnings
^^^^^^^^
Compiler warnings which should be enabled or disabled in the current project:

.. code-block:: json

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

optimizations
^^^^^^^^^^^^^
Compiler optimizations which should be enabled or disabled in the current project:

.. code-block:: json

    {
        "optimizations": {
            "static-type-inference": true,
            "static-type-inference-second-pass": true,
            "local-context-pass": false
        }
    }

globals
^^^^^^^
Extension globals available. Check the :doc:'extension globals <globals>' chapter for more information.

.. code-block:: json

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

info
^^^^
phpinfo() sections. Check the :doc:'phpinfo() <phpinfo>' chapter for more information.

.. code-block:: json

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

initializers
^^^^^^^^^^^^
This setting lets you provide one or more C functions to be executed on certain extension lifecycle events - specifically,
''GINIT'' (''globals''), ''MINIT'' (''module''), and ''RINIT'' (''request''). Check the :doc:'lifecycle hooks
<lifecycle>' chapter for more information.

.. code-block:: json

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

destructors
^^^^^^^^^^^
This setting lets you provide one or more C functions to be executed on certain extension lifecycle events - specifically,
''RSHUTDOWN'' (''request''), ''PRSHUTDOWN'' (''post-request''), ''MSHUTDOWN'' (''module''), and ''GSHUTDOWN'' (''globals'').
Check the :doc:'lifecycle hooks <lifecycle>' chapter for more information.

.. code-block:: json

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

extra-cflags
^^^^^^^^^^^^
Any additional flags you want to add to the compilation process:

.. code-block:: json

    {
        "extra-cflags": "-I/usr/local/Cellar/libevent/2.0.21_1/include"
    }

extra-libs
^^^^^^^^^^
Any additional libraries you want to add to the compilation process:

.. code-block:: json

    {
        "extra-libs": "-L/usr/local/Cellar/libevent/2.0.21_1/lib -levent"
    }

extra-sources
^^^^^^^^^^^^^
Any additional files you want to add to the compilation process - the search directory is relative to the :code:'ext' folder
of your project:

.. code-block:: json

    {
        "extra-sources": [
            "utils/pi.c"
        ]
    }

optimizer-dirs
^^^^^^^^^^^^^^
The directories where your own optimizers can be found - the search directory is relative to the root folder of your project:

.. code-block:: json

    {
        "optimizer-dirs": [
            "optimizers"
        ]
    }

constants-sources
^^^^^^^^^^^^^^^^^
To import just the constants in a C source file into your project, list the file's path in this setting:

.. code-block:: json

    {
        "constants-sources": [
            "utils/math_constants.h"
        ]
    }

extra-classes
^^^^^^^^^^^^^
If you already have a PHP class implmented in C, you can include it directly in your extension by configuring it here:

.. code-block:: json

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

external-dependencies
^^^^^^^^^^^^^^^^^^^^^
You can include a class from another namespace/extension directly in your own extension by configuring it here:

.. code-block:: json

    {
        "external-dependencies": {
            "My\\Awesome": "my/awesome/class.zep",
            "My\\Awful": "my/awful/class.zep"
        }
    }

package-dependencies
^^^^^^^^^^^^^^^^^^^^
Declare library dependencies (version constraints will be checked by :code:'pkg-config', and can use one of the operators
:code:'=', :code:'>=', :code:'<=', or :code:'*'):

.. code-block:: json

    {
        "package-dependencies": {
            "openssl": "*",
            "libpng": ">= 0.1.0",
            "protobuf": "<= 2.6.1"
        }
    }

requires
^^^^^^^^
Allows you to list other extensions as required to build/use your own:

.. code-block:: json

    {
        "requires": {
            "extensions": [
                "igbinary",
                "session"
            ]
        }
    }

prototype-dir
^^^^^^^^^^^^^
Allows you to provide prototype files describing other extensions required to build your own, so they don't necessarily need
to be installed during the build phase:

.. code-block:: json

    {
        "prototype-dir": {
            "igbinary": "prototypes",
            "session": "prototypes"
        }
    }

stubs
^^^^^
This setting allows adjusting the way IDE documentation stubs are generated. :code:'path' sets where the stubs should be
created, while :code:'stubs-run-after-generate' sets whether to automatically (re)build the stubs when your code is compiled
to C:

.. code-block:: json

    {
        "stubs": {
            "path": "ide/%version%/%namespace%/",
            "stubs-run-after-generate": false
        }
    }

api
^^^
Used to configure the automatically generated HTML documentation for your extension. :code:'path' specifies where to create
the documentation relative to the project root. :code:'base-url' is used to generate a :code:'sitemap.xml' file for your
documentation. :code:'theme' is used to set the theme used for the generated documentation (via the :code:'name' setting),
and any options the theme supports passing (via the :code:'options' setting). Finally, :code:'theme-directories' is used to
provide additional search paths for finding your desired theme.:

.. code-block:: json

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

backend
^^^^^^^
Provides a way to configure the Zend Engine backend used by your extension. At the moment, only the :code:'templatepath',
which lets you select between :code:'ZendEngine2' and :code:'ZendEngine3', is supported:

.. code-block:: json

    {
        "backend": {
            "templatepath": "ZendEngine3"
        }
    }

extra
^^^^^
Contains extra settings that also can be passed, as is, on the command line. Currently, that's :code:'export-clases'
(generate headers for accessing your classes from other C code), and :code:'indent' (select between using :code:'tabs' or
:code:'spaces' to indent code in generated files):

.. code-block:: json

    {
        "extra": {
            "export-classes": true,
            "indent": "tabs"
        }
    }

silent
^^^^^^
Suppresses most/all output from :code:'zephir' commands (same as :code:'-w'):

.. code-block:: json

    {
        "silent": false
    }

verbose
^^^^^^^
Displays more detail in error messages from exceptions generated by :code:'zephir' commands (can also enable with :code:'-v',
or disable with :code:'-V'):

.. code-block:: json

    {
        "verbose": false
    }
