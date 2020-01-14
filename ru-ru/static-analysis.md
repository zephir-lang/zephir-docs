---
layout: default
language: 'ru-ru'
version: '0.12'
---

# Статический анализ

Zephir's compiler provides static analysis of the compiled code. Идея этой функции заключается в том, чтобы помочь разработчику найти потенциальные проблемы и избежать неожиданного поведения задолго до времени исполнения.

<a name='conditional-unassigned-variables'></a>

## Условные неинициализированные переменные

Static Analysis of assignments tries to identify if a variable is used before it's assigned:

```zephir
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
```

Приведенный выше пример иллюстрирует общую ситуацию. The variable `a` is assigned only when `b` is equal to 10, then it's required to use the value of this variable - but it could be uninitialized. Zephir detects this, automatically initializes the variable to an empty string, and generates a warning alerting the developer:

```bash
Warning: Variable 'a' was assigned for the first time in conditional branch,
consider initialize it in its declaration in
/home/scott/test/test/utils.zep on 21 [conditional-initialization]

    for c in a {
```

Обнаружить такие ошибки иногда сложно, однако статический анализ помогает программисту обнаружить ошибки заранее.

<a name='dead-code-elimination'></a>

## Удаление мёртвого кода

Zephir информирует разработчика о недоступных ветвях в коде и выполняет удаление мертвого кода, это означает, что он избавляется от всего этого кода из сгенерированного двоичного файла, поскольку он никогда не сможет быть запущен:

```zephir
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
```