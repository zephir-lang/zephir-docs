# 例外情况

Zephir以非常低的级别实现异常，为PHP提供类似的行为和功能。

抛出异常时，可以使用` catch `块来捕获异常并允许开发人员提供正确的处理：

##### 可以在` try `块内抛出异常。 处理发生在` catch `块中，与PHP完全相同。

    var e;
    try {
    
        throw new \Exception("This is an exception");
    
    } catch \Exception, e {
    
        echo e->getMessage();
    }
    

Zephir还提供了一个“silent”` try `块，它只是忽略该块中产生的任何异常：

    try {
        throw new \Exception("This is an exception");
    }
    

如果你在'catch'ing时不需要异常变量，那么你可以放心地不提供它：

    try {
    
        throw new \Exception("This is an exception");
    
    } catch \Exception {
    
        echo "An exception occur!";
    }
    

一个简单的` catch `块可用于捕获多种类型的异常：

    var e;
    try {
    
        throw new \Exception("This is an exception");
    
    } catch \RuntimeException|\Exception, e {
    
        echo e->getMessage();
    }
    

Zephir允许您抛出文字或静态类型变量，就像它们是异常的消息一样：

##### throw new \Exception("Test");

    throw "Test";
    

##### throw new \Exception((string) 't');

    throw 't';
    

##### throw new \Exception((string) 123);

    throw 123;
    

##### throw new \Exception((string) 123.123);

    throw 123.123;
    

Zephir的异常提供了相同的方法来知道PHP异常发生的异常发生的位置。 也就是说，` Exception::getFile()</ 0>和<code> Exception::getLine()</ 0>返回抛出异常的Zephir代码中的位置：</p>

<pre><code>例外：模型'机器人'上不存在静态方法'someMethod'
File=phalcon/mvc/model.zep Line=4042
#0 /home/scott/test.php(64): Phalcon\Mvc\Model::__callStatic('someMethod', Array)
#1 /home/scott/test.php(64): Robots::someMethod()
#2 {main}
`</pre>