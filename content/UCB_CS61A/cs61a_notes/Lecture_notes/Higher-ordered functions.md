---
tags:
  - cs61a
  - python
  - higher-order-functions
  - functions
---

# Iteration Example

### The Fibonacci Sequence

---

# Control

### If statements and call expressions

**Statements（语句）**：一点点往下运行

**Conditional statement 的结构：**

```
if <if header expression>:
    <if suite>
else:
    <else suite>
```

**Execution Rule for Conditional Statements：**

Each clause is considered in order.

1. Evaluate the header's expression (if present)
2. If it is a true value (or an else header), execute the suite & skip the remaining clauses

**Call expressions（调用表达式）**：要先把 operand 运行了再套进去

**Call expression 的结构（对比 if）：**

```
if_(<if header expression>, <if suite>, <else suite>)
```

（这个函数 `if_` 实际并不存在，只是用来类比）

**Evaluation Rule for Call Expressions：**

1. Evaluate the operator and then the operand subexpressions
2. Apply the function that is the value of the operator to the arguments that are the values of the operands

```
def if_(c, t, f):
    if c:
        return t
    else:
        return f
```

由于 call expression 会先对所有 operand 求值（包括 `t` 和 `f` 两个分支），如果直接用 `if_` 代替 `if` 语句：

```python
from math import sqrt

def real_sqrt(x):
    """Return the real part of the square root of x."""
    if x >= 0:
        return sqrt(x)
    else:
        return 0
```

```
~/lec $ python3 -i ex.py
>>> sqrt(16)
4.0
>>> sqrt(-16)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: math domain error
>>> real_sqrt(16)
4.0
>>> real_sqrt(-16)
0
```

因为先运行了 `sqrt(-16)` 所以报错：如果把 `real_sqrt` 改写成用 `if_` 函数（call expression）实现：

```python
def if_(c, t, f):
    if c:
        return t
    else:
        return f

from math import sqrt

def real_sqrt(x):
    """Return the real part of the square root of x."""
    return if_(x >= 0, sqrt(x), 0)
```

```
~/lec $ python3 -i ex.py
>>> sqrt(16)
4.0
>>> sqrt(-16)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: math domain error
>>> real_sqrt(16)
4.0
>>> real_sqrt(-16)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "ex.py", line 11, in real_sqrt
    return if_(x >= 0, sqrt(x), 0)
ValueError: math domain error
```

因为 call expression 会**提前对所有 operand（包括 `sqrt(x)`）求值**，即使 `x >= 0` 是 False，`sqrt(x)` 依然会被执行，所以还是会报错。这说明 `if` 语句和用函数模拟的 `if_` 并不等价。

---

# Control expressions

### Logical operators

**To evaluate the expression `<left> and <right>`：**

1. Evaluate the subexpression `<left>`
2. If the result is a false value `v`, then the expression evaluates to `v`
3. Otherwise, the expression evaluates to the value of the subexpression `<right>`

**To evaluate the expression `<left> or <right>`：**

1. Evaluate the subexpression `<left>`
2. If the result is a true value `v`, then the expression evaluates to `v`
3. Otherwise, the expression evaluates to the value of the subexpression `<right>`

可以让函数不 crash：

```python
def has_big_sqrt(x):
    return x > 0 and sqrt(x) > 0
```

这样的话就算 x 是个负数，也不会出现 error（因为 `and` 是短路求值，`x > 0` 为 False 时不会再去求值 `sqrt(x)`）。

---

# Higher-order functions

```python
def sum_naturals(n):
    """Sum the first N natural numbers.

    >>> sum_naturals(5)
    15
    """
    total, k = 0, 1
    while k <= n:
        total, k = total + k, k + 1
    return total

def sum_cubes(n):
    """Sum the first N cubes of natural numbers.

    >>> sum_cubes(5)
    225
    """
    total, k = 0, 1
    while k <= n:
        total, k = total + pow(k, 3), k + 1
    return total
```

basically the same，**generalization**：这两个函数几乎一模一样，只有累加的项不同，说明可以把"累加的项怎么算"抽象成一个参数。

### summation example

```python
def cube(k):
    return pow(k, 3)

def summation(n, term):
    """Sum the first n terms of a sequence.

    >>> summation(5, cube)
    225
    """
    total, k = 0, 1
    while k <= n:
        total, k = total + term(k), k + 1
    return total
```

- `cube(k)`：Function of a single argument（not called `term`）
- `summation(n, term)` 里的 `term`：A formal parameter that will be bound to a function
- `summation(5, cube)`：The `cube` function is passed as an argument value
- `term(k)`：The function bound to `term` gets called here

---

## Related

- [[Functions]]
- [[Environments]]
- [[Recursion]]

---

# 截图

![[截屏2026-01-24 16.53.52.png]]

![[截屏2026-01-24 16.56.13.png]]

![[截屏2026-01-24 16.56.46.png]]

![[截屏2026-01-24 16.59.27.png]]

![[截屏2026-01-24 18.33.23.png]]

![[截屏2026-01-24 18.38.57.png]]
