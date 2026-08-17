---
tags:
  - cs61a
  - oop
  - representation
  - data-abstraction
---

# String representations

## The repr String for an Object

`repr` 函数返回一个 Python 表达式（字符串），这个表达式求值后会得到一个相等的对象：

```
repr(object) -> string
```

Return the canonical string representation of the object.
For most object types, `eval(repr(object)) == object`.

`repr` 作用在一个值上的结果，就是 Python 在交互式会话里打印出来的东西：

```python
>>> 12e12
12000000000000.0
>>> print(repr(12e12))
12000000000000.0
```

有些对象没有一个简单的、Python 可读的字符串：

```python
>>> repr(min)
'<built-in function min>'
```

## repr 与 str 的对比

终端实测（对字符串 `s` 求 `repr` / `str` / `eval`）：

```python
>>> s = "Hello, World"
>>> s
'Hello, World'
>>> print(repr(s))
'Hello, World'
>>> print(s)
Hello, World
>>> print(str(s))
Hello, World
>>> str(s)
'Hello, World'
>>> repr(s)
"'Hello, World'"
>>> eval(repr(s))
'Hello, World'
>>> repr(repr(repr(s)))
'\'"\\\'Hello, World\\\'"\''
>>> eval(eval(eval(repr(repr(repr(s))))))
'Hello, World'
>>> eval(s)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<string>", line 1, in <module>
NameError: name 'Hello' is not defined
```

- **`repr(obj)`**：给程序员看的，追求**准确**，最好能够还原对象。
- **`str(obj)`**：给用户看的，追求**易读**。

```python
s = "HEllo\nWorld"
print(s)
print()
print(str(s))
print()
print(repr(s))
```

---

# String interpolation

f-strings

终端实测（`f'...'` 中 `{}` 内的表达式会被求值再插入）：

```python
>>> f'2 + 2 = 2 + 2'
'2 + 2 = 2 + 2'
>>> f'2 + 2 = {2 + 2}'
'2 + 2 = 4'
>>> '2 + 2 = {2 + 2}'
'2 + 2 = {2 + 2}'
>>> f'2 + 2 = {abs(2 + 2)}'
'2 + 2 = 4'
>>> abs
<built-in function abs>
>>> abs = float
>>> f'2 + 2 = {abs(2 + 2)}'
'2 + 2 = 4.0'
>>> f'2 + 2 = {(lambda x: x + x)(2)}'
'2 + 2 = 4'
```

用 `Fraction` 的例子：

```python
>>> from fractions import Fraction
>>> half = Fraction(1, 2)
>>> half
Fraction(1, 2)
>>> print(half)
1/2
>>> f'half of a half is {half * half}.'
'half of a half is 1/4.'
>>> f'half of a half is {repr(half * half)}.'
'half of a half is Fraction(1, 4).'
```

---

# polymorphic functions

**同一个函数可以作用于多种不同类型的数据，而不需要为每种类型单独写一个版本。**

"Poly" 表示**多**，"morphic" 表示**形式**，所以 polymorphic 就是**多种形式**。

例：`len()` 就是一个 polymorphic function，同一个 `len()`：
- 可以用于字符串
- 可以用于列表
- 可以用于元组
- 可以用于字典

## Implementing repr and str

`repr` 的行为比直接调用参数上的 `__repr__` 要复杂一点：

- 实例属性（instance attribute）里的 `__repr__` 会被忽略！只有 class attribute 才会被找到。
- **Question**: How would we implement this behavior?

```python
def repr(x):
    return x.__repr__(x)      # ✋ 1

def repr(x):
    return x.__repr__()       # ✌️ 2

def repr(x):
    return type(x).__repr__(x)   # 🖖 3   <- 正确答案
```

`str` 的行为也是类似的复杂：
- 实例属性里的 `__str__` 会被忽略
- 如果没有找到 `__str__` 属性，就使用 `repr` 字符串

**Only class attributes can implement repr.**

### Interfaces （接口）

**Message passing**：对象之间通过在彼此身上查找属性（传递消息）来交互。

- attribute look-up 的规则使得不同的数据类型可以对同一个消息作出响应。
- 一个**共享的消息**（属性名）如果能从不同的对象类里引出相似的行为，就是一种强大的抽象方法。
- **interface（接口）**是一组共享消息，以及它们各自含义的规范说明。

例：实现了 `__repr__` 和 `__str__` 方法（分别返回 Python 可解析的字符串和人类可读的字符串）的类，就实现了一个"生成字符串表示"的接口。

`Ratio` 类（`__init__` / `__repr__` / `__str__`）：

```python
class Ratio:
    def __init__(self, n, d):
        self.numer = n
        self.denom = d

    def __repr__(self):
        return 'Ratio({0}, {1})'.format(self.numer, self.denom)

    def __str__(self):
        return '{0}/{1}'.format(self.numer, self.denom)
```

终端实测：

```python
~/lec$ python3 -i ex.py
>>> half = Ratio(1, 2)
>>> print(half)
1/2
>>> half
Ratio(1, 2)
```

---

# Special Method Names in Python

特殊方法名（以两个下划线开头和结尾）有内置行为：

| 特殊方法 | 作用 |
|---|---|
| `__init__` | 对象被构造时自动调用的方法 |
| `__repr__` | 把对象显示为一个 Python 表达式时调用的方法 |
| `__add__` | 把一个对象加到另一个对象上时调用的方法 |
| `__bool__` | 把对象转换为 `True` 或 `False` 时调用的方法 |
| `__float__` | 把对象转换为 float（实数）时调用的方法 |

```python
>>> zero, one, two = 0, 1, 2
>>> one + two
3
>>> bool(zero), bool(one)
(False, True)
```

上面这两行和下面两行行为完全一样（"Same behavior using methods"）：

```python
>>> zero, one, two = 0, 1, 2
>>> one.__add__(two)
3
>>> zero.__bool__(), one.__bool__()
(False, True)
```

完整的 `Ratio` 类，加上了 `__add__` / `__radd__` / `__float__` 和一个 `gcd` 辅助函数：

```python
class Ratio:
    def __init__(self, n, d):
        self.numer = n
        self.denom = d

    def __repr__(self):
        return 'Ratio({0}, {1})'.format(self.numer, self.denom)

    def __str__(self):
        return '{0}/{1}'.format(self.numer, self.denom)

    def __add__(self, other):
        if isinstance(other, int):
            n = self.numer + self.denom * other
            d = self.denom
        elif isinstance(other, Ratio):
            n = self.numer * other.denom + self.denom * other.numer
            d = self.denom * other.denom
        elif isinstance(other, float):
            return float(self) + other
        g = gcd(n, d)
        return Ratio(n//g, d//g)

    __radd__ = __add__

    def __float__(self):
        return self.numer/self.denom

def gcd(n, d):
    while n != d:
        n, d = min(n, d), abs(n-d)
    return n
```

终端实测：

```python
~/lec$ python3 -i ex.py
>>> 0.2 + Ratio(1,3)
0.5333333333333333
```

---

## Related

- [[Data Abstraction]]
- [[Object-Oriented Programming]]
- [[Attributes]]

---

# 截图

![[截屏2026-07-08 15.14.54.png]]

![[截屏2026-07-08 15.15.31.png]]

![[截屏2026-07-08 15.25.38.png]]

![[截屏2026-07-08 15.26.10.png]]

![[截屏2026-07-08 15.45.34.png]]

![[截屏2026-07-08 15.46.57.png]]

![[截屏2026-07-08 15.44.57.png]]

![[截屏2026-07-08 16.16.58.png]]

![[截屏2026-07-08 16.17.22.png]]
