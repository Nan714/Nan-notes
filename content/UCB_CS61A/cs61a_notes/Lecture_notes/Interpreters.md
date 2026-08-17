---
tags:
  - cs61a
  - scheme
  - interpreters
---

# The Structure of an Interpreter

**Eval** — base cases:
- Primitive values (numbers)
- Look up values bound to symbols

**Eval** — recursive calls:
- `Eval(operator, operands)` of call expressions
- `Apply(procedure, arguments)`
- `Eval(sub-expressions)` of special forms

**Apply** — base cases:
- Built-in primitive procedures

**Apply** — recursive calls:
- `Eval(body)` of user-defined procedures

- Eval requires an environment for symbol lookup
- Applying a user-defined procedure creates a new environment each time

---

# Special Forms

## Scheme Evaluation

The `scheme_eval` function dispatches on expression form:
- Symbols are bound to values in the current environment.
- Self-evaluating expressions are returned.
- All other legal expressions are represented as Scheme lists, called *combinations*.

```scheme
(if <predicate> <consequent> <alternative>)
(lambda (<formal-parameters>) <body>)
(define <name> <expression>)
(<operator> <operand 0> ... <operand k>)
```

- Special forms are identified by the first list element.
- Any combination that is not a known special form is a call expression.

```scheme
(define (demo s) (if (null? s) '(3) (cons (car s) (demo (cdr s)))))
(demo (list 1 2))
```

---

# Logical forms

## Logical Special Forms

Logical forms may only evaluate some sub-expressions.

- **If** expression: `(if <predicate> <consequent> <alternative>)`
- **And** and **or**: `(and <e1> ... <en>)`, `(or <e1> ... <en>)`
- **Cond** expr'n: `(cond (<p1> <e1>) ... (<pn> <en>) (else <e>))`

The value of an **if** expression is the value of a sub-expression (`do_if_form`, part of `scheme_eval`):
1. Evaluate the predicate.
2. Choose a sub-expression: `<consequent>` or `<alternative>`.
3. Evaluate that sub-expression in place of the whole expression.

终端 trace 示例：

```
scm> (if #t 1 2)
scheme_eval(<(if True 1 2)>, <Global Frame>):
    scheme_eval(True, <Global Frame>):
    scheme_eval(True, <Global Frame>) -> True
    scheme_eval(1, <Global Frame>):
    scheme_eval(1, <Global Frame>) -> 1
scheme_eval(<(if True 1 2)>, <Global Frame>) -> 1
1
scm> (if #t 1 (/ 1 0))
scheme_eval(<(if True 1 (/ 1 0))>, <Global Frame>):
    scheme_eval(True, <Global Frame>):
    scheme_eval(True, <Global Frame>) -> True
    scheme_eval(1, <Global Frame>):
    scheme_eval(1, <Global Frame>) -> 1
scheme_eval(<(if True 1 (/ 1 0))>, <Global Frame>) -> 1
1
scm> (if #f 1 (/ 1 0))
scheme_eval(<(if False 1 (/ 1 0))>, <Global Frame>):
    scheme_eval(False, <Global Frame>):
    scheme_eval(False, <Global Frame>) -> False
    scheme_eval(<(/ 1 0)>, <Global Frame>):
        scheme_eval('/', <Global Frame>):
        scheme_eval('/', <Global Frame>) -> #[/]
        scheme_eval(1, <Global Frame>):
        scheme_eval(1, <Global Frame>) -> 1
        scheme_eval(0, <Global Frame>):
        scheme_eval(0, <Global Frame>) -> 0
        scheme_eval exited via exception
    scheme_eval exited via exception
Traceback (most recent call last):
  0     (/ 1 0)
Error: division by zero
scm>
```

**这个判断和 python 的 T/F and or 很像。如果 if 跟着 true，就直接走 true 那边，就算 alternative 那边有 error，也不会报错，因为就不会走那边**

---

# Quotation

evaluate to…… 求值后得到…… or 计算结果是……

The **quote** special form evaluates to the quoted expression, which is **not** evaluated.

```scheme
(quote <expression>)
(quote (+ 1 2))     ; evaluates to the three-element Scheme list (+ 1 2)
```

The `<expression>` itself is the value of the expression.

`'<expression>` is shorthand for `(quote <expression>)`.

```scheme
(quote (1 2))   ; is equivalent to  '(1 2)
```

**The scheme_read parser converts shorthand to a combination.**

---

# Lambda expressions

## Lambda Expressions

Lambda expressions evaluate to user-defined procedures.

```scheme
(lambda (<formal-parameters>) <body>)
(lambda (x) (* x x))
```

```python
class LambdaProcedure:
    def __init__(self, formals, body, env):
        self.formals = formals  # A scheme list of symbols
        self.body = body        # A scheme expression
        self.env = env          # A Frame instance
```

## Frames and Environments

- A frame represents an environment by having a parent frame.
- Frames are Python instances with methods **lookup** and **define**.
- In Project 4, Frames do not hold return values.

```
g: Global frame
    y | 3
    z | 5

f1: [parent=g]
    x | 2
    z | 4
```

终端示例：

```python
>>> 2+2
4
>>> Frame
<class '__main__.Frame'>
>>> scheme_eval
<function scheme_eval at 0x1011c80e0>
>>> g = Frame(None)
>>> g
<Global Frame>
>>> f1 = Frame(g)
>>> f1
<{} -> <Global Frame>>
>>> g.define('y', 3)
>>> g.define('z', 5)
>>> g.lookup('y')
3
>>> g.lookup('z')
5
>>> f1.define('x', 2)
>>> f1.define('z', 4)
>>> f1
<{x: 2, z: 4} -> <Global Frame>>
>>> f1.lookup('x')
2
>>> f1.lookup('z')
4
>>> f1.lookup('y')
3
```

**define 就是定义，lookup 就是查找。**
**这个 frame 和 python 一样意思**

---

# Define expressions

## Define Expressions

Define binds a symbol to a value in the first frame of the current environment.

```scheme
(define <name> <expression>)
```

1. Evaluate the `<expression>`.
2. Bind `<name>` to its value in the current frame.

```scheme
(define x (+ 1 2))
```

Procedure definition is shorthand of define with a lambda expression.

```scheme
(define (<name> <formal parameters>) <body>)

(define <name> (lambda (<formal parameters>) <body>))
```

**倒数两行 这两行完全 equivalent**

---

# Dynamic scope

## Dynamic Scope

The way in which names are looked up in Scheme and Python is called *lexical scope* (or *static scope*).

- **Lexical scope**: The parent of a frame is the environment in which a procedure was *defined*.
- **Dynamic scope**: The parent of a frame is the environment in which a procedure was *called*.

```scheme
(define f (lambda (x) (+ x y)))
(define g (lambda (x y) (f (+ x x))))
(g 3 7)
```

- **Lexical scope**: The parent for f's frame is the global frame. → `Error: unknown identifier: y`
- **Dynamic scope**: The parent for f's frame is g's frame. → `13`

lexical scope 就是在哪里 defined 的
dynamic scope 就是在哪里被 called 的

---

## Related

- [[Scheme]]
- [[Calculator]]
- [[Macros]]

---

# 截图

![[截屏2026-07-31 14.03.22.png]]

![[截屏2026-07-31 14.15.38.png]]

![[截屏2026-07-31 14.25.00.png]]

![[截屏2026-07-31 14.25.18.png]]

![[截屏2026-07-31 14.30.14.png]]

![[截屏2026-07-31 14.34.29.png]]

![[截屏2026-07-31 14.37.41.png]]

![[截屏2026-07-31 14.37.55.png]]

![[截屏2026-07-31 14.58.54.png]]

![[截屏2026-07-31 15.07.46.png]]
