类和对象
===================
Zephir只支持面向对象编程，这也是为什么在Zephir代码出错时我们使用异常而非致命错误的原因。

类
-------
每个Zephir文件必须有一个类或接口且只能有一个。类的结构近似于PHP：

.. code-block:: zephir

    namespace Test;

    /**
     * This is a sample class
     */
    class MyClass
    {

    }

类修饰符
^^^^^^^^^^^^^^^
Zephir中支持如下的类修饰符:

*Final*: 如果一个类标识为final则这个类不能被扩展

.. code-block:: zephir

    namespace Test;

    /**
     * 这个类不能被扩展
     */
    final class MyClass
    {

    }

*Abstract*: 如果一个类标识为Abstract则这个类不能被实例化

.. code-block:: zephir

    namespace Test;

    /**
     * 这个类不能被实例化
     */
    abstract class MyClass
    {

    }

类中的方法
--------------------
Zephir中的方法使用关键字function定义。方法必须提供可见性修饰符，Zephir中的方法对PHP代码亦有可见性:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        public function myPublicMethod()
        {
            // ...
        }

        protected function myProtectedMethod()
        {
            // ...
        }

        private function myPrivateMethod()
        {
            // ...
        }
    }

方法支持必填与可选参数:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        /**
         * 所有参数都是必填的
         */
        public function doSum1(a, b)
        {
            return a + b;
        }

        /**
         * 只有a是必填的因b有默认值则其是可选的
         */
        public function doSum2(a, b = 3)
        {
            return a + b;
        }

        /**
         * 两个参数都是可选的
         */
        public function doSum3(a = 1, b = 2)
        {
            return a + b;
        }

        /**
         * 必填的整型参数
         */
        public function doSum4(int a, int b)
        {
            return a + b;
        }

        /**
         * 静态类型参数默认值
         */
        public function doSum4(int a = 4, int b = 2)
        {
            return a + b;
        }
    }

null默认值的可选参数 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Zephir编译器可以保证静态类型参数的类型不被改变（这是必须滴），因此当有默认的null值时该null值会被转换成适当的值：

.. code-block:: zephir

    public function foo(int a = null)
    {
        echo a; // 如果未填写参数则a被转换为 0 并输出
    }

    public function foo(boolean a = null)
    {
        echo a; // 如果未填写参数则a被转换为 false 并输出
    }

    public function foo(string a = null)
    {
        echo a; // 如果未填写参数则a被转换为空字符串并输出
    }

    public function foo(array a = null)
    {
        var_dump(a); // 如果未填写参数则a被转换为空数组并输出
    }

支持的可见性
^^^^^^^^^^^^^^^^^^^^^^

* Public(公有): 当方法被标记为public时，意味着该方法对PHP扩展与PHP代码来说都是可见的。 

* Protected(保护): 当方法被标记为protected的话，其对PHP扩展与PHP代码都提供了可见性，但限于其继承者（Zephir类与PHP类）。

* Private: 当方法被标记为private意味着，该方法不向任何非定义方法类之外的类暴露其访问权限。

支持的修饰符
^^^^^^^^^^^^^^^^^^^

* Final: 如果方法被标记为final则该方法不能被重写

* Deprecated: 如果方法被标记为deprecated，则此方法在调用时会抛出E_DEPRECATED异常

Getter/Setter快捷生成器
^^^^^^^^^^^^^^^^^^^^^^^
像C#中一样Zephir提供子get/set/toString三个快捷生成器，使用快捷生成器的话我们可以不需要像如下代码一样手动的书写访问方法：

例如没有快捷生成器的话我们要这样写代码:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {
        protected myProperty;

        protected someProperty = 10;

        public function setMyProperty(myProperty)
        {
            let this->myProperty = myProperty;
        }

        public function getMyProperty()
        {
            return this->myProperty;
        }

        public function setSomeProperty(someProperty)
        {
            let this->someProperty = someProperty;
        }

        public function getSomeProperty()
        {
            return this->someProperty;
        }

        public function __toString()
        {
            return this->myProperty;
        }

     }

有快捷生成器的话可以这样写:

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        protected myProperty {
            set, get, toString
        };

        protected someProperty = 10 {
            set, get
        };

    }

当上述代码被编译时，Zephir编译器才会生成真正的访问器代码，这让我们省了不少事。

返回类型
^^^^^^^^^^^^^^^^^
类中的方法可以书写返回类型（也可以不写），如果书写返回类型的话则编译器就会有足够的信息来帮助我们找到更多潜在的bug。考虑如下代码：

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        public function getSomeData() -> string
        {
            //这里会在编译时抛出异常，因为我们设定的返回类型为string而这里是false
            return false;
        }

        public function getSomeOther() -> <App\MyInterface>
        {
            //如果App\MyObject未实现App\MyInterface接口的话则这里会在编译时抛出异常
            return new App\MyObject;
        }

        public function process()
        {
            var myObject;

            //getSomeOther的方法的返回类型暗示了myObject的类型为App\MyInterface的实现类的一个实例
            let myObject = this->getSomeOther();

            //编译器会检测App\MyInterface中是否有someMethod方法
            echo myObject->someMethod();
        }

    }

方法可以有多个返回参数类型。当有多个返回类型时，要使用|来分割不同的返回类型。

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        public function getSomeData(a) -> string | bool
        {
            if a == false {
                return false;
            }
            return "error";
        }
    }

void返回值类型
^^^^^^^^^^^^^^^^^
方法可以返回void。如果方法标记为返回void时，则此方法就不能再返回任何数据了:

.. code-block:: zephir

    public function setConnection(connection) -> void
    {
        let this->_connection = connection;
    }

这很有用？当然了，这会让编译器检测返回值类型然后决定是否抛出异常，这会让我们的代码有更少的bug:

.. code-block:: zephir

    let myDb = db->setConnection(connection);
    myDb->execute("SELECT * FROM robots"); // 这里会抛出异常

严格/宽松参数类型检查
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Zephir中开发者可以为每个参数指定类型。默认情况下参数的检查是宽松的，这意味着如果传递的实参类型不对时（但兼容），
Zephir的编译器会自动的将其进行转换成期望的类型:


.. code-block:: zephir

    public function filterText(string text, boolean escape=false)
    {
        //...
    }

上述代码在调用时的表现:

.. code-block:: php

    <?php

    $o->filterText(1111, 1); // OK
    $o->filterText("some text", null); // OK
    $o->filterText(null, true); // OK
    $o->filterText("some text", true); // OK
    $o->filterText(array(1, 2, 3), true); // FAIL

当然了，传递不正确的类型的参数通常会导致bug。调用API时如果使用了错误的实参类型通常会导致不期的结果。
当然你可以通过使用严格的类型检查。

.. code-block:: zephir

    public function filterText(string! text, boolean escape=false)
    {
        //...
    }

现在，下面不匹配的实参传递通常会导致异常:

.. code-block:: php

    <?php

    $o->filterText(1111, 1); // FAIL
    $o->filterText("some text", null); // OK
    $o->filterText(null, true); // FAIL
    $o->filterText("some text", true); // OK
    $o->filterText(array(1, 2, 3), true); // FAIL

通过设定参数类型是严格或宽松检查的，开发者可以很好的控制自己的代码行为以期实现自己所求。

只读参数 
^^^^^^^^^^^^^^^^^^^^
Zephir支持只读参数，在参数前加const关键字即可。更多信息参见 `const-correctness <http://en.wikipedia.org/wiki/Const-correctness>`_.


.. code-block:: zephir

    namespace App;

    class MyClass
    {
        // "a"是只读参数 
        public function getSomeData(const string a)
        {
            // 编译时抛出异常
            let a = "hello";
        }
    }

当一个方法被写成只读时编译器会生成更安全的代码且可以执行更近一步的性能优化。

类属性
-----------------------
类的成员变量即是类属性。默认情况下类属性的表现与PHP类属性一样。Zephir中的类属性对PHP扩展与PHP代码有可见性。
Zephir类属性的可见性访问修饰符是必须的:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        public myProperty1;

        protected myProperty2;

        private myProperty3;

    }

Zephir类内需要使用->来访问类属性:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected myProperty;

        public function setMyProperty(var myProperty)
        {
            let this->myProperty = myProperty;
        }

        public function getMyProperty()
        {
            return this->myProperty;
        }
    }

类属性可以有初值。这些初值是在编译时确定的而不是运行时:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected myProperty1 = null;
        protected myProperty2 = false;
        protected myProperty3 = 2.0;
        protected myProperty4 = 5;
        protected myProperty5 = "my value";
    }

更新属性值
^^^^^^^^^^^^^^^^^^^
属性值可以使用->来进行访问近而完成更新操作:

.. code-block:: zephir

    let this->myProperty = 100;

Zephir会检查类属性是否存在如果不存在则会抛出编译异常:

.. code-block:: php

    CompilerException: Property '_optionsx' is not defined on class 'App\MyClass' in /Users/scott/utils/app/myclass.zep on line 62

          let this->_optionsx = options;
          ------------^

如果你想忽略编译检查而动态生成类属性，可以使用如下的方式来做:

.. code-block:: zephir

    let this->{"myProperty"} = 100;

可以通过如下的中间变量来更新类属性:

.. code-block:: zephir

    let someProperty = "myProperty";
    let this->{someProperty} = 100;

读取类属性值
^^^^^^^^^^^^^^^^^^
类属性值可以使用->来完成读取操作:

.. code-block:: zephir

    echo this->myProperty;

更新过后可以使用如下的方法来动态的读取属性值:

.. code-block:: zephir

    //动态的读取属性值（不使用编译时检查）
    echo this->{"myProperty"};

    //使用中间变量来读取属性值
    let someProperty = "myProperty";
    echo this->{someProperty}

类常量
---------------
Zephir类中可以定义类常量。类常量可以在Zephir类中或是PHP代码中来使用。

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        const MYCONSTANT1 = false;
        const MYCONSTANT2 = 1.0;
    }

类常量要使用::进行访问:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        const MYCONSTANT1 = false;
        const MYCONSTANT2 = 1.0;

        public function someMethod()
        {
            return MyClass::MYCONSTANT1;
        }
    }

调用方法
---------------
Zephir中的方法可以像PHP一样使用->来进行调用:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected function _someHiddenMethod(a, b)
        {
            return a - b;
        }

        public function someMethod(c, d)
        {
            return this->_someHiddenMethod(c, d);
        }
    }

静态方法要使用::进行调用:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected static function _someHiddenMethod(a, b)
        {
            return a - b;
        }

        public static function someMethod(c, d)
        {
            return self::_someHiddenMethod(c, d);
        }
    }

可以像如下代码一样动态的调用方法:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {
        protected adapter;

        public function setAdapter(var adapter)
        {
            let this->adapter = adapter;
        }

        public function someMethod(var methodName)
        {
            return this->adapter->{methodName}();
        }
    }

具名参数 
^^^^^^^^^^^^^^^^^^
Zephir的方法支持具名参数方法调用，具名参数方法调用非常有用，尤其是在你想使用任何实参序时，而且也可以给开发者更多的提示信息。

考虑如下代码，Image类有一个接收4个参数的方法:

.. code-block:: zephir

    namespace Test;

    class Image
    {
        public function chop(width=600, height=400, x=0, y=0)
        {
            //...
        }
    }

标准调用方式:

.. code-block:: zephir

    i->chop(100); // width=100, height=400, x=0, y=0
    i->chop(100, 50, 10, 20); // width=100, height=50, x=10, y=20

使用具名参数:

.. code-block:: zephir

    i->chop(width: 100); // width=100, height=400, x=0, y=0
    i->chop(height: 200); // width=600, height=200, x=0, y=0
    i->chop(height: 200, width: 100); // width=100, height=200, x=0, y=0
    i->chop(x: 20, y: 30); // width=600, height=400, x=20, y=30

编译器在编译时不知道参数序，参数序需要在运行时才会确定，这样一来有一点不好的地方即是性能会有所损失:

.. code-block:: zephir

    let i = new {someClass}();
    i->chop(y:30, x: 20);
