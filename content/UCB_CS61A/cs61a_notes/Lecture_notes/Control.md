---
tags:
  - cs61a
  - python
  - control-flow
---

# Multiple Environments

## Life Cycle of a User-Defined Function

**Def statement:**

```python
def square(x):
    return mul(x, x)
```

- `square` — Name，`x` — Formal parameter，`mul(x, x)` — Return expression
- 整个 `def` 语句是 Def statement，函数体（`return` 语句）是 Body

**What happens?**
- A new function is created!
- Name bound to that function in the current frame

**Call expression:**

```python
square(2 + 2)
```

- operator: `square`；operand: `2 + 2`
- operator 求值为 function: `func square(x)`；operand 求值为 argument: `4`

**What happens?**
- Operator & operands evaluated
- Function (value of operator) called on arguments (values of operands)

**Calling/Applying:**

- `4` 是 Argument，绑定到 Signature `square(x)`，Return value 是 `16`

**What happens?**
- A new frame is created!
- Parameters bound to arguments
- Body is executed in that new environment

## Names Have No Meaning Without Environments

```python
from operator import mul
def square(x):
    return mul(x, x)
square(square(3))
```

- Every expression is evaluated in the context of an environment.
- A name evaluates to the value bound to that name in the earliest frame of the current environment in which that name is found.

环境图：
- Global frame: `mul` → `func mul(...)`，`square` → `func square(x) [parent=Global]`
- `f1: square [parent=Global]`：`x = 3`，`Return value = 9`（对应内层 `square(3)`）
- `f2: square [parent=Global]`：`x = 9`，`Return value = 81`（对应外层 `square(square(3))`）

**An environment is a sequence of frames.**

先找 earliest 的，再慢慢往后推

- The global frame alone
- A local, then the global frame

## Names Have Different Meanings in Different Environments

```python
from operator import mul
def square(square):
    return mul(square, square)
square(4)
```

环境图：
- Global frame: `mul` → `func mul(...)`，`square` → `func square(square) [parent=Global]`
- `f1: square [parent=Global]`：`square = 4`，`Return value = 16`
- Every expression is evaluated in the context of an environment.
- A name evaluates to the value bound to that name in the earliest frame of the current environment in which that name is found.

**A call expression and the body of the function being called are evaluated in different environments.**

---

# Conditional Statements

## Statements

A *statement* is executed by the interpreter to perform an action.

**Compound statements:**

```
<header>:
    <statement>
    <statement>
    ...
<separating header>:
    <statement>
    <statement>
    ...
...
```

- The first header determines a statement's type
- The header of a clause "controls" the suite that follows
- `def` statements are compound statements

A *suite* is a sequence of statements

To *execute* a suite means to execute its sequence of statements, in order

Execute rule for a sequence of statements:
- Execute the first statement
- Unless directed otherwise, execute the rest

## Conditional Statements

```python
def absolute_value(x):
    """Return the absolute value of x."""
    if x < 0:
        return -x
    elif x == 0:
        return 0
    else:
        return x
```

这个例子里：1 statement, 3 clauses, 3 headers, 3 suites

**Execution rule for conditional statements:**

Each clause is considered in order.
1. Evaluate the header's expression.
2. If it is a true value, execute the suite & skip the remaining clauses.

**Syntax Tips**
1. Always starts with "if" clause.
2. Zero or more "elif" clauses.
3. Zero or one "else" clause, always at the end.

## Boolean Contexts

False: `False`, `0`, `''`, `None` (more to come)（第三个是空的 string）

True: Anything else (`True`)

---

# Iteration

## While Statements

```python
i, total = 0, 0
while i < 3:
    i = i + 1
    total = total + i
```

（Demo，George Boole 头像作为背景引入布尔逻辑）

执行到某一步时环境：Global frame `i = 3`，`total = 6`

**Execution Rule for While Statements:**
1. Evaluate the header's expression.
2. If it is a true value, execute the (whole) suite, then return to step 1.

## Related

- [[Sequence]]
- [[Functions]]
- [[Recursion]]

---

# 截图

![[截屏2026-01-23 17.21.37.png]]

![[截屏2026-01-23 17.34.08.png]]

![[截屏2026-01-23 17.39.28.png]]

![[截屏2026-01-23 17.44.11.png]]

![[截屏2026-01-23 17.50.15.png]]

![[截屏2026-01-23 17.56.44.png]]
