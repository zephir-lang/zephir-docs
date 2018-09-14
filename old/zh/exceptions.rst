异常
==========
Zephir在底层实现了一个类似于PHP异常功能的异常（机制）。

当异常被抛出时时，开发者可以在"catch"块中对异常情况进行处理。

.. code-block:: zephir

    //这里必须定义异常变量否则会出错
    val e; 
    
    try {

        // 抛出异常
        throw new \Exception("This is an exception");

    } catch \Exception, e {

        // 处理异常
        echo e->getMessage();
    }

Zephir提供了一个静默的异常处理，即不对异常进行处理（忽略异常的处理）:

.. code-block:: zephir

    try {
    
        throw new \Exception("This is an exception");
    }

在处理异常时如果你不需要异常变量可以不进行定义，直接不写即可:

.. code-block:: zephir
	
    try {

        // 抛出异常
        throw new \Exception("This is an exception");

    } catch \Exception {

        echo "这里是一个异常，但没有使用异常变量";
    }


在一个"catch"块中可以捕获多种类型的异常：

.. code-block:: zephir
	
    var e;
    try {

        // 抛出异常
        throw new \Exception("This is an exception");

    } catch \RuntimeException|\Exception, e {

        // 处理异常	
        echo e->getMessage();
    }

在Zephir中我们可以抛出字符串或是静态类型的变量，他们最终会被当作字符串进行处理（不能放入try块中否则编译不能过）:

.. code-block:: zephir

    throw "Test"; // throw new \Exception("Test");
    throw 't'; // throw new \Exception((string) 't');
    throw 123; // throw new \Exception((string) 123);
    throw 123.123; // throw new \Exception((string) 123.123);


Zephir异常与PHP异常非常类似，比如我们可以找到异常发生的位置。Exception::getFile() 和 Exception::getLine()返回当异常发生时的文件与行号: 

.. code-block:: html

    Exception: The static method 'someMethod' does not exist on model 'Robots'
    File=phalcon/mvc/model.zep Line=4042
    #0 /home/scott/test.php(64): Phalcon\Mvc\Model::__callStatic('someMethod', Array)
    #1 /home/scott/test.php(64): Robots::someMethod()
    #2 {main}
