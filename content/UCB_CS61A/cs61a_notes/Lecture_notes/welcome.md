---
tags:
  - cs61a
  - expressions
---

# Expressions

## Types of Expressions

An expression describes a computation and evaluates to a value.

例如：`18 + 69`，`sin pi`，`f(x)`

## Anatomy of a Call Expression

```
add    (1+1    ,    3)
operator   operand      operand
function        argument
```

Evaluation procedure for call expressions：

1. evaluate the operator and then the operand subexpressions
2. apply the function that is the value of the operator subexpression to the arguments that are the values of the operand subexpressions

**Expression tree 示例**：`mul(add(2, mul(4, 6)), add(3, 5))`

- 最内层：`mul(4, 6)` → `24`
- `add(2, 24)` → `26`（value of subexpression / 1st argument to `mul`）
- `add(3, 5)` → `8`
- 最外层：`mul(26, 8)` → `208`（value of the whole expression）

一个函数总是求值成一个值，而这个值 26 乘以 8 就得到了 208，整个表达式的值。

从下到上一步步往上推。

## Assignment Statements

```python
x = 1 + 2
```

## Related

- [[Functions]]
- [[Recursion]]

---

# 截图

![[截屏2026-01-19 08.08.51.png]]
