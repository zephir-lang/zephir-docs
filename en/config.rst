Configuration File
==================
Every Zephir extension has a configuration file called :code:`config.json`. This file is read by Zephir every time you build
or generate the extension, and it allows the developer to modify the extension's or compiler's behavior.

This file uses `JSON <http://en.wikipedia.org/wiki/JSON>`_ as its configuration format:

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
The namespace of the extension - it must be a simple identifier respecting the regular expression :code:`[a-zA-Z0-9\_]+`:

.. code-block:: json

    {
        "namespace": "test"
    }

name
^^^^
Extension name - can only contain ascii characters:

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
Extension version - must follow the regular expression :code:`[0-9]+\.[0-9]+\.[0-9]+`:

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
Extension globals available. Check the :doc:`extension globals <globals>` chapter for more information.

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
phpinfo() sections. Check the :doc:`phpinfo() <phpinfo>` chapter for more information.

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
Any additional files you want to add to the compilation process - the search directory is relative to the :code:`ext` folder
of your project:

.. code-block:: json

    {
        "extra-sources": ["utils/pi.c"]
    }

optimizer-dirs
^^^^^^^^^^^^^^
The directories where your own optimizers can be found - the search directory is relative to the root folder of your project:

.. code-block:: json

    {
        "optimizer-dirs": ["optimizer-dirs"]
    }

package-dependencies
^^^^^^^^^^^^^^^^^^^^
Declare library dependencies (version constraints will be checked by :code:`pkg-config`, and can use one of the operators
:code:`=`, :code:`>=`, :code:`<=`, or :code:`*`):

.. code-block:: json

    {
        "package-dependencies": {
            "openssl": "*",
            "libpng": ">= 0.1.0",
            "protobuf": "<= 2.6.1"
        }
    }
