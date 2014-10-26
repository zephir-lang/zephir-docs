Exceptions
==========
Zephir implements exceptions at a very low level providing a similar behavior like PHP
but in compiled code.

When an exception is thrown, a "catch" block can caught the exception and the
developer can provide the proper handling.

.. code-block:: zephir

    try {

        // exceptions can be thrown here
        throw new \Exception("This is an exception");

    } catch \Exception, e {

        // handle exception
        echo e->getMessage();
    }

Zephir provides a silent "try" block that simply ignores any exception produced within the block:

.. code-block:: zephir

    try {
        throw new \Exception("This is an exception");
    }

When catching the exception if you don't need a variable for the exception you can safely miss it:

.. code-block:: zephir

    try {

        // exceptions can be thrown here
        throw new \Exception("This is an exception");

    } catch \Exception {

        // handle exception
        echo e->getMessage();
    }


A single "catch" block can be used to catch many types of exceptions:

.. code-block:: zephir

    try {

        // exceptions can be thrown here
        throw new \Exception("This is an exception");

    } catch RuntimeException|Exception, e {

        // handle exception
        echo e->getMessage();
    }

Zephir allows you to throw literals or static typed variables as they were the message of the exception:

.. code-block:: zephir

    throw "Test"; // throw new \Exception("Test");
    throw 't'; // throw new \Exception((string) 't');
    throw 123; // throw new \Exception((string) 123);
    throw 123.123; // throw new \Exception((string) 123.123);

Zephir's exceptions provides the same facilities as in PHP to know where exception happened.
Exception::getFile() and Exception::getLine() returns the location in the Zephir code
where the exception has been thrown:

.. code-block:: html

    Exception: The static method 'someMethod' doesn't exist on model 'Robots'
    File=phalcon/mvc/model.zep Line=4042
    #0 /home/scott/test.php(64): Phalcon\Mvc\Model::__callStatic('someMethod', Array)
    #1 /home/scott/test.php(64): Robots::someMethod()
    #2 {main}