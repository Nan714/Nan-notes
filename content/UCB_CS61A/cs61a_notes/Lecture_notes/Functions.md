---
tags:
  - cs61a
  - python
  - functions
---

# Names, Assignment, and User-Defined Functions

- terminal: `python3`
- `control + L`：clear（清屏）

bind names to values（把名字绑定到值的三种方式）：

1. `import`（内置好的名字）
2. assignment statement（赋值语句）
3. `def` statement（创建新函数）

---

# Environment Diagrams

python tutor：可以看到过程（procedure）的执行细节

### Execution rule for assignment statements

1. Evaluate all expressions to the right of `=` from left to right
2. Bind all names to the left of `=` to the resulting values in the current frame

```python
a = 1
b = 2
b, a = a + b, b
print(a, b)
```

先把 `a+b`、`b` 的结果算出来，再将结果带入 `b`、`a`。

---

# Defining functions

```python
def <name>(<formal parameter>):
    return <return expression>
```

### Execution procedure for def statements

1. Create a function with signature `<name>(<formal parameters>)`
2. Set the body of that function to be everything indented after the first line
3. Bind `<name>` to that function in the current frame

### Procedure for calling/applying user-defined functions (version 1)

1. Add a local frame, forming a *new* environment
2. Bind the function's formal parameters to its arguments in that frame
3. Execute the body of the function in that new environment

```python
from operator import mul

def square(x):
    return mul(x, x)

square(-2)
```

环境图：

```
Global frame
    mul    -----> func mul(...)          (Built-in function)
    square -----> func square(x)         (User-defined function)

Local frame (square)
    x            -2
    Return value  4
```

- `Original name of function called`：局部帧标题记录的是被调用函数的原始名字
- `Formal parameter bound to argument`：形参 `x` 被绑定为实参 `-2`
- `Return value (not a binding!)`：返回值不是一次绑定，只是记录在帧里的结果

A function's signature has all the information needed to create a local frame.

### Looking up names in environments

Every expression is evaluated in the context of an environment. So far, the current environment is either:

- the global frame alone, or
- a local frame, followed by the global frame

### 2 important things

1. An environment is a sequence of frames
2. A name evaluates to the value bound to that name in the earliest frame of the current environment in which that name is found（先 local frame，再 global frame）

#environment

---

# Print and None

### None indicates that nothing is returned

- **None** represents **nothing** in Python
- A function that does not explicitly return a value will return **None**
- *Careful*：**None** is not displayed by the interpreter as the value of an expression

```python
def does_not_square(x):
    x * x  # No return

does_not_square(4)  # None value is not displayed
sixteen = does_not_square(4)
sixteen + 4
# TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
```

### Pure functions & Non-Pure functions

**Pure Functions**（just return values）

```
-2        -> abs -> 2                       # Return value
2, 100    -> pow -> 126765060022822940...   # 2 Arguments
```

**Non-Pure Functions**（have side effects）

```
-2 -> print -> None      # Returns None!
```

Python displays the output `"-2"`（side effect），但函数本身返回的是 `None`。A side effect isn't a value; it's anything that happens as a consequence of calling a function。

### Nested expressions with print

```python
>>> print(print(1), print(2))
1
2
None None
```

求值过程：`print(print(1), print(2))` 先分别求值两个子表达式 `print(1)` 和 `print(2)`：

- `print(1)` 先 display "1"，然后返回 `None`
- `print(2)` 先 display "2"，然后返回 `None`
- 最外层 `print(None, None)` 再 display "None None"（这次的 `None None` 不会被再次显示，因为外层调用的返回值本身也是 `None`，interpreter 不显示 `None`）

#none

---

# Miscellaneous Python Features

```python
add()
mul()
from operator import truediv  # /
from operator import floordiv  # //

def divide_exact(n, d=10):
    ...
```

`d` 不填数字的话默认是 10。

## Related

- [[Composition]]
- [[Decomposition]]
- [[Higher-ordered functions]]
- [[Environments]]

---

# 截图

![[截屏2026-01-18 23.36.19.png]]

![[截屏2026-01-19 07.55.10.png]]

![[截屏2026-01-19 07.58.33.png]]
