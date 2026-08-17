---
tags:
  - cs61a
  - scheme
  - macros
---

# Accessing Nested Lists

```scheme
lst = (a b c)
; 想要取到 c
(car (cdr (cdr lst)))
```

外层的 car 不能忘！不加 car 的话取出来的就是 `(c)`，是个 list，加上 car 就是 `c`，是这个元素本身

---

# Expressions

## Discussion Question: Pythagorean Theorem

Quick quasiquotation review: `` `(+ ,(* 2 3) 1) `` evaluates to `(+ 6 1)`.

Add `` ` `` and `,` in some blanks so that the second expression evaluates to `(+ (* a a) (* b b))`:

```scheme
(define (square-expr term) `(* ,term ,term))

`(+ ,(square-expr `a) ,(square-expr `b))
```

反引号中可以用逗号来进行运算

---

# Macros

## Macros Perform Code Transformations

A macro is an operation performed on the source code of a program before evaluation.

Macros exist in many languages, but are easiest to define correctly in a language like Lisp.

Scheme has a **define-macro** special form that defines a source code transformation:

```scheme
(define-macro (twice expr)
  (list 'begin expr expr))

> (twice (print 2))
2
2
▶ (begin (print 2) (print 2))
```

Evaluation procedure of a macro call expression:
- Evaluate the operator sub-expression, which evaluates to a macro
- Call the macro procedure on the operand expressions *without evaluating them first*
- Evaluate the expression returned from the macro procedure

这个就是先不 evaluate 先 call，然后最后再 evaluate

终端示例：

```scheme
~/lec$ ./scheme
Welcome to the CS 61A Scheme Interpreter (version 1.2.2)
scm> (print 2)
2
scm> (define x (print 2))
2
x
scm> x
scm> (begin (print 2) (print 2))
2
2
scm> (define (twice expr) (list 'begin expr expr))
twice
scm> (twice (print 2))
2
(begin None None)
scm> (twice '(print 2))
(begin (print 2) (print 2))
scm> (eval (twice '(print 2)))
2
2
scm> (define-macro (twice expr) (list 'begin expr expr))
twice
scm> (twice (print 2))
2
2
scm>
```

**define**

	如果是define的话 后面的expr如果不括号括起来的话，就会先evaluate，导致print 2变为None；
	所以define 需要将expr括起来，但这个时候依旧不会运算；
	想要运算的话，用eval

**define-macro**

	define-macro就会直接运算， macro其实就是scheme里定义各种函数的东西，让内置函数跑起来
	用list（）是在造代码写出来代码，而不是直接（）运行代码

---
# For Macro

## Discussion Question

Define a macro that evaluates an expression for each value in a sequence.

```scheme
(define (map fn vals)
  (if (null? vals)
      ()
      (cons (fn (car vals))
            (map fn (cdr vals)))))

scm> (map (lambda (x) (* x x)) '(2 3 4 5))
(4 9 16 25)

(define-macro (for sym vals expr)
  (list 'map (list 'lambda (list sym) expr) vals))

scm> (for x '(2 3 4 5) (* x x))
(4 9 16 25)
```

看到 list 了吗！要用起来！
lambda 后面跟的 symbol 一定是一个 list！

---

# Trace

## Tracing Recursive Calls

左边的是 decorator：

```python
@decorator
def greet():
    print("Hello!")

# 这两个是等价的！

def greet():
    print("Hello!")

greet = decorator(greet)
```

```python
def trace(fn):
    def traced(n):
        print(f'{fn.__name__}({n})')
        return fn(n)
    return traced

@trace
def fact(n):
    if n == 0:
        return 1
    else:
        return n * fact(n - 1)

>>> fact(5)
fact(5)
fact(4)
fact(3)
fact(2)
fact(1)
fact(0)
120
```

**其实就是 fact 变成了 wrapper（traced），通过 greet = decorator(greet)（提示: return traced）可以看出**

右边 scheme 的逻辑就是先给出 fact 函数，再让 original 成为 fact，然后再重新定义 fact。所以 original 就是原来的 fact，fact 就是新定义的。

```scheme
(define fact (lambda (n)
  (if (zero? n) 1 (* n (fact (- n 1))))))

(define original fact)
(define fact (lambda (n)
              (print (list 'fact n))
              (original n)))

scm> (fact 5)
(fact 5)
(fact 4)
(fact 3)
(fact 2)
(fact 1)
(fact 0)
120
```

用 `define-macro` 把上面的模式包成通用的 `trace`：

```scheme
~/lec $ ./scheme ex.scm
Welcome to the CS 61A Scheme Interpreter (version 1.2.5)
scm> (define fact (lambda (n)
        (if (zero? n) 1 (* n (fact (- n 1))))))
fact
scm> (fact 5)
120
scm> (define-macro (trace expr) ; (trace (fact 5))
        (define operator (car expr)) ; fact
      `(begin
         (define original ,operator)
         (define ,operator (lambda (n)
                              (print (list ,operator n))
                              (original n)))
         (define result ,expr)
         (define ,operator original)
         result))
trace
scm> (trace (fact 5))
((lambda (n) (print (list fact n)) (original n)) 5)
((lambda (n) (print (list fact n)) (original n)) 4)
((lambda (n) (print (list fact n)) (original n)) 3)
((lambda (n) (print (list fact n)) (original n)) 2)
((lambda (n) (print (list fact n)) (original n)) 1)
((lambda (n) (print (list fact n)) (original n)) 0)
120
scm> (fact 5)
120

~/lec $
```

如果是 `(quote ,operator)` 那么就会直接给出 fact，因为从最开始的 define（红色）来看，operator 是 `(car expr)` 就是 fact：

```scheme
(define-macro (trace expr) ; (trace (fact 5))
  (define operator (car expr)) ; fact
 `(begin
    (define original ,operator)
    (define ,operator (lambda (n)
                         (print (list (quote ,operator) n))
                         (original n)))
    (define result ,expr)
    (define ,operator original)
    result))

(trace (fact 5))

(fact 5)
```

---

## Related

- [[Scheme]]
- [[Interpreters]]
- [[Programming as data]]

---

# 截图

![[截屏2026-08-01 14.57.36.png]]

![[截屏2026-08-03 11.32.01.png]]

![[截屏2026-08-03 11.36.11.png]]

![[截屏2026-08-03 12.00.34.png]]

![[截屏2026-08-03 15.18.10.png]]

![[截屏2026-08-03 15.50.55.png]]

![[截屏2026-08-03 15.24.59.png]]
