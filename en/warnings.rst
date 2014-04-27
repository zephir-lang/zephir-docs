Compiler Warnings
=================

The compiler raise warnings when it finds situations where the code can be improved or a potential error
can be avoided.

Warnings can be enabled via command line parameters or can be added to the config.json to enable or disable
them permanently:

You can enable warnings by passing its name prefixed by -w:

.. code-block:: bash

    zephir -wunused-variable -wnonexistent-function

Warnings can be disabled by passing its name prefixed by -W:

.. code-block:: bash

    zephir -Wunused-variable -Wnonexistent-function

The following warnings are supported:

unused-variable
^^^^^^^^^^^^^^^
Raised when a variable is declared but it is not used within a method. This warning is enabled by default.

.. code-block:: zephir

    public function some()
    {
        var e; // declared but not used

        return false;
    }

unused-variable-external
^^^^^^^^^^^^^^^^^^^^^^^^
Raised when a parameter is declared but it is not used within a method.

.. code-block:: zephir

    public function sum(a, b, c) // c is not used
    {
        return a + b;
    }

possible-wrong-parameter-undefined
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Raised when a method is called with a wrong type for a parameter:

.. code-block:: zephir

    public function some()
    {
        return this->sum("a string", "another");  // wrong parameters passed
    }

    public function sum(int a, int b)
    {
        return a + b;
    }

nonexistent-function
^^^^^^^^^^^^^^^^^^^^
Raised when a non-existent function at compile time is called:

.. code-block:: zephir

    public function some()
    {
        someFunction(); // someFunction does not exist
    }

nonexistent-class
^^^^^^^^^^^^^^^^^
Raised when a non-existent class is used at compile time:

.. code-block:: zephir

    public function some()
    {
        var a;

        let a = new \MyClass(); // MyClass does not exist
    }

non-valid-isset
^^^^^^^^^^^^^^^
Raised when the compiler detects that an 'isset' operation is being made on a non array or object value:

.. code-block:: zephir

    public function some()
    {
        var b = 1.2;
        return isset b[0]; // variable integer 'b' used as array
    }

non-array-update
^^^^^^^^^^^^^^^^
Raised when the compiler detects that an array update operation is being made on a non array value:

.. code-block:: zephir

    public function some()
    {
        var b = 1.2;
        let b[0] = true; // variable 'b' cannot be used as array
    }

non-valid-objectupdate
^^^^^^^^^^^^^^^^^^^^^^
Raised when the compiler detects that an object update operation is being made on a non object:

.. code-block:: zephir

    public function some()
    {
        var b = 1.2;
        let b->name = true; // variable 'b' cannot be used as object
    }

non-valid-fetch
^^^^^^^^^^^^^^^
Raised when the compiler detects that a 'fetch' operation is being made on a non array or object value:

.. code-block:: zephir

    public function some()
    {
        var b = 1.2, a;
        fetch a, b[0]; // variable integer 'b' used as array
    }

invalid-array-index
^^^^^^^^^^^^^^^^^^^
Raised when the compiler detects that an invalid array index is used:

.. code-block:: zephir

    public function some(var a)
    {
        var b = [];
        let a[b] = true;
    }

non-array-append
^^^^^^^^^^^^^^^^
Raised when the compiler detects that an element is being tried to be appended to a non array variable:

.. code-block:: zephir

    public function some()
    {
        var b = false;
        let b[] = "some value";
    }

non-array-append
^^^^^^^^^^^^^^^^
Raised when the compiler detects that an element is being tried to be appended to a non array variable:

.. code-block:: zephir

    public function some()
    {
        var b = false;
        let b[] = "some value";
    }

            'invalid-return-type'                => true,
            'unreachable-code'                   => true,
            'nonexistant-constant'               => true,
            'not-supported-magic-constant'       => true,
            'non-valid-decrement'                => true,
            'non-valid-increment'                => true,
            'non-valid-clone'                    => true,
            'non-valid-new'                      => true,
            'non-array-access'                   => true,
            'invalid-reference'                  => true,
            'invalid-typeof-comparison'          => true,
            'conditional-initialization'         => true