---
tags:
  - cs61a
  - python
  - environments
---

# Lambda Expressions

```python
>>> x = 10
>>> square = x * x
>>> square = lambda x: x * x
>>> square(4)
16
```

- `x = 10` 是一个表达式，求值为一个数字
- `square = x * x` 也是一个表达式，但这里改写为 `square = lambda x: x * x`，同样是一个表达式，只不过求值结果是一个**函数**
- `lambda x: x * x` 是一个函数：
  - **A function**（`lambda`）
  - **with formal parameter x**（`x`，形参）
  - **that returns the value of "x * x"**（`x * x`，函数体表达式）
- **重要**：lambda 表达式没有 `return` 关键字，函数体只能是**单个表达式**
- Lambda expressions are not common in Python, but important in general
- Lambda expressions in Python **cannot contain statements at all**！

## Lambda Expressions Versus Def Statements

```python
square = lambda x: x * x        # VS        def square(x):
                                 #               return x * x
```

- 两者都创建了一个拥有相同 domain、range 和 behavior 的函数
- 两个函数的 parent 都是它们被定义时所在的 frame
- 两者都把这个函数绑定到名字 `square`
- 只有 `def` 语句会给函数一个**内在名字（intrinsic name）**

环境图对比：

**lambda 版本**

```
Global frame
    square -----> func λ(x) <line 1> [parent=Global]

f1: λ <line 1> [parent=Global]
    x            4
    Return value 16
```

（`λ` 是希腊字母 lambda）

**def 版本**

```
Global frame
    square -----> func square(x) [parent=Global]

f1: square [parent=Global]
    x            4
    Return value 16
```

两边的局部帧结构完全一样，唯一区别是函数本身有没有名字（`λ` vs `square`）。

---

# Function Currying

**Currying**：Transforming a multi-argument function into a single-argument, higher-order function.
把一个接受多个参数的函数，转换成一系列每次只接受一个参数的函数。

```python
def make_adder(n):
    return lambda k: n + k

m = make_adder(2)
print(m(3))
print(m(5))
```

## Related

- [[Functions]]
- [[Higher-ordered functions]]
- [[Interpreters]]

---

# 截图

![[截屏2026-06-22 12.54.40.png]]

![[截屏2026-06-22 12.58.16.png]]
