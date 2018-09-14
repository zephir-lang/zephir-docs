编译警告
=================

编译器在检测到有潜在的错误或可提升性能的地方时会报警告给开发者。

我们可以使用命令行参数打开或关闭编译警告也可以在config.json配置文件中永久的打开或关闭编译警告:

编译时在警告名前加-w来实现打开警告:

.. code-block:: bash

    zephir -wunused-variable -wnonexistent-function

编译时在警告名前加-W来关闭警告:

.. code-block:: bash

    zephir -Wunused-variable -Wnonexistent-function

Zephir支持如下的编译警告:

unused-variable
^^^^^^^^^^^^^^^
当前一个变量定义后未使用时就会报这个警告。这个警告默认是开启的。


.. code-block:: zephir

    public function some()
    {
        var e; // 声明但未使用

        return false;
    }

unused-variable-external
^^^^^^^^^^^^^^^^^^^^^^^^
当方法的参数未使用时报这个警告:

.. code-block:: zephir

    public function sum(a, b, c) // c is not used
    {
        return a + b;
    }

possible-wrong-parameter-undefined
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
调用方法时如果参数类型错误则会报这个警告:

.. code-block:: zephir

    public function some()
    {
        return this->sum("a string", "another");  // 传递了错误的参数
    }

    public function sum(int a, int b)
    {
        return a + b;
    }

nonexistent-function
^^^^^^^^^^^^^^^^^^^^
调用不存在的方法时报这个警告:

.. code-block:: zephir

    public function some()
    {
        someFunction(); // 函数someFunction不存在
    }

nonexistent-class
^^^^^^^^^^^^^^^^^
编译时发现不存在的类时报这个警告:

.. code-block:: zephir

    public function some()
    {
        var a;

        let a = new \MyClass(); // 类MyClass不存在
    }

non-valid-isset
^^^^^^^^^^^^^^^^^^^
当编译器检测到isset指令作用于非数组或对象时报这个警告:

.. code-block:: zephir

    public function some()
    {
        var b = 1.2;
        return isset b[0]; // b作为数组使用了
    }

non-array-update
^^^^^^^^^^^^^^^^
当编译器检测到对非数组进行更新时报这个警告:

.. code-block:: zephir

    public function some()
    {
        var b = 1.2;
        let b[0] = true; // 变量'b'不是array
    }

non-valid-objectupdate
^^^^^^^^^^^^^^^^^^^^^^
当编译器检测到对一个非对象执行更新时报这个警告:

.. code-block:: zephir

    public function some()
    {
        var b = 1.2;
        let b->name = true; // 变量'b'不是一个对象
    }

non-valid-fetch
^^^^^^^^^^^^^^^
当编译器检测到对一个非对象或数组进行fetch操作时报这个警告:

.. code-block:: zephir

    public function some()
    {
        var b = 1.2, a;
        fetch a, b[0]; //变量b作为数组使用了
    }

invalid-array-index
^^^^^^^^^^^^^^^^^^^
当编译器检测到错误的数组索引时报这个警告:

.. code-block:: zephir

    public function some(var a)
    {
        var b = [];
        let a[b] = true;
    }

non-array-append
^^^^^^^^^^^^^^^^
当编译器检测到一个元素试图追加到一个非数组时报这个警告:

.. code-block:: zephir

    public function some()
    {
        var b = false;
        let b[] = "some value";
    }
