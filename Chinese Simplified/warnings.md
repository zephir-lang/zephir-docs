# 编译警告

当发现代码可以改进或避免潜在错误的情况时，编译器会发出警告。

警告可以通过命令行参数启用，也可以添加到`config.json`以更永久地启用或禁用它们。

可以通过将警告的名称以`-w`为前缀传递来启用警告:

    zephir -wunused-variable -wnonexistent-function
    

警告可以通过传递前缀为`-W`的名称来禁用:

    zephir -Wunused-variable -Wnonexistent-function
    

支持以下警告:

<a name='unused-variable'></a>

## unused-variable

在声明变量但在方法中未使用时引发。 此警告默认启用。

    public function some()
    {
        var e; // declared but not used
    
        return false;
    }
    

<a name='unused-variable-external'></a>

## unused-variable-external

当一个参数被声明但在方法中没有使用时引发。

    public function sum(a, b, c) // c is not used
    {
        return a + b;
    }
    

<a name='possible-wrong-parameter-undefined'></a>

## possible-wrong-parameter-undefined

当一个方法以错误的类型调用参数时引发:

    public function some()
    {
        return this->sum("a string", "another");  // wrong parameters passed
    }
    
    public function sum(int a, int b)
    {
        return a + b;
    }
    

<a name='nonexistent-function'></a>

## nonexistent-function

调用编译时不存在的函数时引发:

    public function some()
    {
        someFunction(); // someFunction does not exist
    }
    

<a name='nonexistent-class'></a>

## nonexistent-class

当使用编译时不存在的类时引发:

    public function some()
    {
        var a;
    
        let a = new \MyClass(); // MyClass does not exist
    }
    

<a name='non-valid-isset'></a>

## non-valid-isset

当编译器检测到正在对非数组或-对象值执行“isset”操作时引发:

    public function some()
    {
        var b = 1.2;
    
        return isset b[0]; // variable integer 'b' used as array
    }
    

<a name='non-array-update'></a>

## non-array-update

当编译器检测到正在对非数组值执行数组更新操作时引发:

    public function some()
    {
        var b = 1.2;
        let b[0] = true; // variable 'b' cannot be used as array
    }
    

<a name='non-valid-objectupdate'></a>

## non-valid-objectupdate

当编译器检测到正在对非对象值进行对象更新操作时引发:

    public function some()
    {
        var b = 1.2;
        let b->name = true; // variable 'b' cannot be used as object
    }
    

<a name='non-valid-fetch'></a>

## non-valid-fetch

当编译器检测到正在对非数组或-对象值进行'fetch'操作时引发:

##### 变量整数'b'用作数组

    public function some()
    {
        var b = 1.2, a;
        fetch a, b[0];
    }
    

<a name='invalid-array-index'></a>

## invalid-array-index

当编译器检测到使用了无效的数组索引时引发:

    public function some(var a)
    {
        var b = [];
        let a[b] = true;
    }
    

<a name='non-array-append'></a>

## non-array-append

当编译器检测到一个元素被附加到一个非数组变量时引发:

    public function some()
    {
        var b = false;
        let b[] = "some value";
    }