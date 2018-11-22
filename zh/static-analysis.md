# 静态分析

Zephir 的编译器提供对已编译代码的静态分析。 此功能背后的想法是在运行时之前帮助开发人员发现潜在的问题, 避免意外的行为 。

<a name='conditional-unassigned-variables'></a>

## 条件未分配的变量

工作分配的静态分析尝试确定变量在赋值之前是否已使用:

    class Utils
    {
        public function someMethod(b)
        {
            string a; char c;
    
            if b == 10 {
                let a = "hello";
            }
    
            //a could be unitialized here
            for c in a {
                echo c, PHP_EOL;
            }
        }
    }
    

The above example illustrates a common situation. The variable `a` is assigned only when `b` is equal to 10, then it's required to use the value of this variable - but it could be uninitialized. Zephir detects this, automatically initializes the variable to an empty string, and generates a warning alerting the developer:

    Warning: Variable 'a' was assigned for the first time in conditional branch,
    consider initialize it in its declaration in
    /home/scott/test/test/utils.zep on 21 [conditional-initialization]
    
        for c in a {
    

Finding such errors is sometimes tricky, however static analysis helps the programmer to find bugs in advance.

<a name='dead-code-elimination'></a>

## Dead Code Elimination

Zephir informs the developer about unreachable branches in the code and performs dead code elimination, which means it gets rid of all that code from the generated binary, since it cannot be executed anyway:

    class Utils
    {
        public function someMethod(b)
        {
            if false {
                // This is never executed
                echo "hello";
            }
        }
    }