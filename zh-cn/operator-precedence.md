---
layout: default
language: 'zh-cn'
version: '0.12'
---

# Operator Precedence
**Operator precedence** determines how operators are parsed. Operators with higher precedence become the operands of operators with lower precedence. For example, in the expression `1 + 5 * 3`, the answer is 16 and not 18 because the multiplication (`*`) operator has a higher precedence than the addition (`+`) operator. Parentheses may be used to force precedence, if necessary. For instance: `(1 + 5) * 3` evaluates to 18.

<a name='operator-precedence-associativity'></a>

## Associativity
When operators have equal precedence their **associativity** determines how the operators are grouped. For example `-` is _left-associative_, so `1 - 2 - 3` is grouped as `(1 - 2) - 3` and evaluates to `-4`. `=` on the other hand is _right-associative_, so `let a = b = c` is grouped as `let a = (b = c)`.

Operators of equal precedence that are _non-associative_ cannot be used next to one another. For example:
```zep
new new Foo();
```
is illegal in Zephir, because the `new` is non-associative. At the moment, `new` is the only non-associative operator in Zephir.

Use of parentheses, even when not strictly necessary, can often increase readability of the code, by making grouping explicit, rather than relying on the implicit operator precedence and associativity.

<a name='operator-precedence-table'></a>

## Precedence Table
The following table lists the operators in order of precedence, with the highest-precedence ones at the top. Operators on the same line have equal precedence, in which case associativity decides grouping.

| Precedence | 运算符                              | Operator type                       | Associativity   |
| ---------- | -------------------------------- | ----------------------------------- | --------------- |
| 1          | `->`                          | Member Access                       | right-to-left   |
| 2          | `~`                              | Bitwise NOT                         | right-to-left   |
| 3          | `!`                              | Logical NOT                         | right-to-left   |
| 4          | `new`                            | new                                 | non-associative |
| 5          | `clone`                          | clone                               | right-to-left   |
| 6          | `typeof`                         | Type-of                             | right-to-left   |
| 7          | `..`, `...`                      | Inclusive/exclusive range           | left-to-right   |
| 8          | `isset`, `fetch`, `empty`        | Exclusive range                     | right-to-left   |
| 9          | `*`, `/`, `%`                    | Multiplication, Division, Remainder | left-to-right   |
| 10         | `+`, `-`, `.`                    | Addition, Subtraction, Concat       | left-to-right   |
| 11         | `<<`, `>>`           | Bitwise shift left/right            | left-to-right   |
| 12         | `<`, `<=`, `=>`, `>` | Comparison                          | left-to-right   |
| 13         | `==`, `!==`, `===`, `!==`        | Comparison                          | left-to-right   |
| 14         | `&`                          | Bitwise AND, references             | left-to-right   |
| 15         | `^`                              | Bitwise XOR                         | left-to-right   |
| 16         | `|`                              | Bitwise OR                          | left-to-right   |
| 17         | `instanceof`                     | Instance-of                         | left-to-right   |
| 18         | `&&`                     | Logical AND                         | left-to-right   |
| 19         | `||`                             | Logical OR                          | left-to-right   |
| 20         | `likely`, `unlikely`             | Branch prediction                   | right-to-left   |
| 21         | `?`                              | Logical                             | right-to-left   |
| 21         | `=>`                          | Closure Arrow                       | right-to-left   |
