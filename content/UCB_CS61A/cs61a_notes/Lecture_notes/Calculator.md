---
tags:
  - cs61a
  - scheme
  - interpreters
  - calculator
---

# Exceptions

## Raise Statements

Python 的异常用 `raise` 语句抛出：

```python
raise <expression>
```

`<expression>` 必须求值为 `BaseException` 的子类，或者是它的一个实例。异常和其他对象一样被构造，例如 `TypeError('Bad argument!')`。

常见异常类型：

- `TypeError` —— 函数被传入了错误数量/类型的参数
- `NameError` —— 找不到某个名字
- `KeyError` —— 字典里找不到某个 key
- `RecursionError` —— 递归调用次数太多

```python
def double(x): 
	raise TypeError('Double only takes numbers')
	2*x
	
double(2)
double('hello') # TypeError
```

### Try statement

`try` 语句用于处理异常：

```python
try:
    <try suite>
except <exception class> as <name>:
    <except suite>
...
```

**执行规则：**

- `<try suite>` 首先被执行
- 如果在执行 `<try suite>` 的过程中抛出了一个未被处理的异常，且这个异常的类继承自 `<exception class>`
- 那么 `<except suite>` 就会被执行，其中 `<name>` 绑定到这个异常对象

终端实际例子：

```python
def invert(x):
    result = 1/x
    print('Never printed if x is 0')
    return result

def invert_safe(x):
    try:
        return invert(x)
    except ZeroDivisionError as e:
        return str(e)
```

```
~/lec $ python3 -i ex.py
>>> a = invert_safe(2)
Never printed if x is 0
>>> a
0.5
>>> b = invert_safe(0)
>>> b
'division by zero'
>>>
```

---

# Example: Reduce

```python
def reduce(f,s,initial):
	"""Combine elements of s using f starting from initial"""
	>>>reduce(mul, [2,4,8],1)
	64
	>>>reduce(add,[1,2,3,4],0)
	10
	"""
	for x in s:
		initial = f(initial,x)
	return initial
#for循环写法

def reduce(f，s，initial):
	if not s:
		return initial
	return reduce(f,s[1:],f(initial,s[0])) #最后的initial负责不断累加/累运算，最后到底了就输出initial，就是最终的结果
```

---

# Programming language

## Programming Languages

计算机通常会执行用许多不同编程语言写的程序。

**Machine languages（机器语言）：** 语句直接被硬件本身解释

- 一组固定的指令，调用 CPU 电路实现的操作
- 操作直接指向具体的硬件内存地址；没有抽象机制

**High-level languages（高级语言）：** 语句和表达式被另一个程序解释，或者被编译（翻译）成另一种语言

- 提供命名、函数定义、对象等抽象手段
- 把系统细节抽象掉，独立于硬件和操作系统

```python
# Python 3
def square(x):
    return x * x
```

```
from dis import dis
dis(square)
```

```
# Python 3 Byte Code
LOAD_FAST                0 (x)
LOAD_FAST                0 (x)
BINARY_MULTIPLY
RETURN_VALUE
```

## Metalinguistic Abstraction

一种强大的抽象方式是定义一门新的语言，专门针对某种应用类型或问题领域。

- **Type of application（应用类型）：** Erlang 是为并发程序设计的，内置了表达并发通信的元素，例如用来实现有大量同时连接的聊天服务器
- **Problem domain（问题领域）：** MediaWiki 标记语言是为生成静态网页设计的，内置了文本格式化和跨页链接的元素，例如用来创建 Wikipedia 页面

一门编程语言有：

- **Syntax（语法）：** 语言中合法的语句和表达式
- **Semantics（语义）：** 这些语句和表达式的执行/求值规则

要创建一门新的编程语言，要么需要：

- **Specification（规范）：** 一份精确描述语言语法和语义的文档
- **Canonical Implementation（标准实现）：** 该语言的一个解释器或编译器

---

# Calculator

scheme_reader.py 是用来读取的
scalc.py 是用来运算的（scalc: scheme-syntax calculator）

终端实际例子：

```
lec/scalc $ python3 scheme_reader.py
lec/scalc $ python3 scheme_reader.py --repl
read> )
SyntaxError: unexpected token: )
read> (+ 1 2)
str : (+ 1 2)
repr: Link('+', Link(1, Link(2)))
read> (* (+ 1 2)   4)
str : (* (+ 1 2) 4)
repr: Link('*', Link(Link('+', Link(1, Link(2))), Link(4)))
read> ^D
lec/scalc $ python3 scalc.py
scm> (* (+ 1 2) 4)
12
scm>
```

```python
if operator == '+':
    return reduce(add, args, 0)
elif operator == '-':
    if args is nil:
        raise TypeError(operator + ' requires at least 1 argument')
    elif args.rest is nil:
        return -args.first
    else:
        return reduce(sub, args.rest, args.first)
elif operator == '*':
    return reduce(mul, args, 1)
elif operator == '/':
    if args is nil:
        raise TypeError(operator + ' requires at least 1 argument')
    elif args.rest is nil:
        return 1 / args.first
    else:
        return reduce(truediv, args.rest, args.first)
else:
    raise TypeError(operator + ' is an unknown operator')

@main
def read_eval_print_loop():
    """Run a read-eval-print loop for Calculator."""
    while True:
        try:
            src = buffer_input()
            while src.more_on_line():
                expression = scheme_read(src)
                print(calc_eval(expression))
        except (SyntaxError, TypeError, ValueError, ZeroDivisionError) as err:
            print(type(err).__name__ + ':', err)
        except (KeyboardInterrupt, EOFError):  # <Control>-D, etc.
            print('Calculation completed.')
            return
```

### Calculator Syntax

Calculator 语言只有 primitive expression 和 call expression（就这些！）。

- **Primitive expression** 是一个数字：`2`  `-4`  `5.6`
- **Call expression** 是一个以操作符（`+`, `-`, `*`, `/`）开头，后面跟 0 个或多个表达式的组合：`(+ 1 2 3)`  `(/ 3 (+ 4 5))`

表达式被表示成 Scheme 的 list（`Link` 实例），编码成树结构。例如 `(* 3 (+ 4 5) (* 6 7 8))` 对应的表达式树以 `*` 为根，三个分支分别是 `3`、`(+ 4 5)`、`(* 6 7 8)`；用 `Link` 对象链表表示为：`Link('*', Link(3, Link(Link('+', Link(4, Link(5))), Link(Link('*', Link(6, Link(7, Link(8)))), nil))))`。

Syntax 只包括语法，但实际上 `+` `-` 等等符号代表的意义（`+` `-` 是做什么的，代表什么）并不属于语法范畴，属于 semantics。

### Calculator semantics

一个 calculator 表达式的值是递归定义的：

- **Primitive：** 一个数字求值为它自身
- **Call：** 一个 call expression 求值为它的参数值经过某个操作符组合后的结果
  - `+`：所有参数的和
  - `*`：所有参数的积
  - `-`：如果只有一个参数，取负；如果有多个参数，用第一个减去其余的和
  - `/`：如果只有一个参数，取倒数；如果有多个参数，用第一个除以其余的积

例如 `(+ 5 (* 2 3) (* 2 5 5))` 的表达式树：根节点 `61`，分支为 `+`、`5`、`6`（来自 `(* 2 3)`）、`50`（来自 `(* 2 5 5)`）。

终端 trace 例子：

```
scm> (+ 5
        (* 2 3)
        (* 2 5 5))
calc_eval(Link('+', Link(5, Link(Link('*', Link(2, Link(3))), Link(Link('*', Link(2, Link(5, Link(5)))))))))：
    calc_eval(5):
    calc_eval(5) -> 5
    calc_eval(Link('*', Link(2, Link(3)))):
        calc_eval(2):
        calc_eval(2) -> 2
        calc_eval(3):
        calc_eval(3) -> 3
        calc_apply('*', Link(2, Link(3))):
        calc_apply('*', Link(2, Link(3))) -> 6
    calc_eval(Link('*', Link(2, Link(3)))) -> 6
    calc_eval(Link('*', Link(2, Link(5, Link(5))))):
        calc_eval(2):
        calc_eval(2) -> 2
        calc_eval(5):
        calc_eval(5) -> 5
        calc_eval(5):
        calc_eval(5) -> 5
        calc_apply('*', Link(2, Link(5, Link(5)))):
        calc_apply('*', Link(2, Link(5, Link(5)))) -> 50
    calc_eval(Link('*', Link(2, Link(5, Link(5))))) -> 50
    calc_apply('+', Link(5, Link(6, Link(50)))):
    calc_apply('+', Link(5, Link(6, Link(50)))) -> 61
calc_eval(...) -> 61
61
scm>
```

```python
from ucb import main, trace
from utils import reduce
from operator import add, sub, mul, truediv
from scheme_reader import scheme_read, buffer_input
from link import Link, nil, map_link

# Eval & Apply

@trace
def calc_eval(exp):
    """Evaluate a Calculator expression.

    >>> calc_eval(as_scheme_list('+', 2, as_scheme_list('*', 4, 6)))
    26
    >>> calc_eval(as_scheme_list('+', 2, as_scheme_list('/', 40, 5)))
    10
    """
    if type(exp) in (int, float):
        return exp
    elif isinstance(exp, Link):
        arguments = map_link(calc_eval, exp.rest)
        return calc_apply(exp.first, arguments)
    else:
        raise TypeError(str(exp) + ' is not a number or call expression')

@trace
def calc_apply(operator, args):
    """Apply the named operator to a list of args.

    >>> calc_apply('+', as_scheme_list(1, 2, 3))
    6
    >>> calc_apply('-', as_scheme_list(10, 1, 2, 3))
    4
    >>> calc_apply('-', as_scheme_list(10))
    -10
    >>> calc_apply('*', nil)
    1
    >>> calc_apply('*', as_scheme_list(1, 2, 3, 4, 5))
    120
    >>> calc_apply('/', as_scheme_list(40, 5))
    8.0
    >>> calc_apply('/', as_scheme_list(10))
    0.1
    ...
    ZeroDivisionError: division by zero
    """
```

**Eval evaluates expressions.**

**Apply applies procedures to arguments.**

也就是说：

- **`eval`（求值）**：把表达式解释成一个值；如果是函数调用，还要先求出函数和所有参数。
- **`apply`（应用）**：拿着已经求好的函数和参数，真正执行这个函数。

```
(+ (* 2    3) 4)
        │
        ▼
      eval
        │
        ├─ eval +
        ├─ eval (* 2 3)
        │        │
        │        ▼
        │      apply *
        │        │
        │        ▼
        │        6
        │
        ├── eval 4
        ▼
函数：+
参数：[6, 4]
        │
        ▼
     apply +
        │
        ▼
        10
```

## Related

- [[Scheme]]
- [[Interpreters]]
- [[Programming as data]]

---

# 截图

![[截屏2026-07-28 16.56.59.png]]

![[截屏2026-07-28 17.08.23.png]]

![[截屏2026-07-28 17.14.27.png]]

![[截屏2026-07-31 11.48.44.png]]

![[截屏2026-07-31 11.51.43.png]]

![[截屏2026-07-31 12.02.27.png]]

![[截屏2026-07-31 12.06.53.png]]

![[截屏2026-07-31 12.09.25.png]]

![[截屏2026-07-31 12.18.36.png]]
