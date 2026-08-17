---
tags:
  - cs61a
  - scheme
  - programming-as-data
---

Scheme Specification 参考：`https://insideempire.github.io/CS61A-Website-Archive/articles/scheme-spec/index.html#let`

# A Scheme Expression is a Scheme List

Scheme 程序由 expression 组成，可以是：

- **Primitive expressions**：`2`  `3.3`  `true`  `+`  `quotient`
- **Combinations**：`(quotient 10 2)`  `(not true)`

Scheme 内置的 list 数据结构（本质是 linked list）可以用来**表示 combination**：

```scheme
scm> (list 'quotient 10 2)
(quotient 10 2)

scm> (eval (list 'quotient 10 2))
5
```

**`'quotient` 这个是让 quotient 不要成为运算符，而是作为标志直接放在这里；`eval` 就会让式子运算，直接得到结果。**

在这样的语言里，很容易写出"写程序的程序"。

## 示例：fact 与 fact-exp

```scheme
(define (fact n)
  (if (= n 0) 1 (* n (fact (- n 1)))))

(define (fact-exp n)
  (if (= n 0) 1 (list '* n (fact-exp (- n 1)))))
```

终端实测：

```
scm> fact
(lambda (n) (if (= n 0) 1 (* n (fact (- n 1)))))
scm> (fact 3)
6
scm> (fact 5)
120
scm> (fact-exp 5)
(* 5 (* 4 (* 3 (* 2 (* 1 1)))))
scm> (eval (fact-exp 5))
120
```

`fact-exp` 不直接计算结果，而是**构造出一个代表计算过程的 list**（表达式），再用 `eval` 对它求值。

## 示例：fib 与 fib-exp

```scheme
(define (fib n)
  (if (<= n 1) n (+ (fib (- n 2)) (fib (- n 1)))))

(define (fib-exp n)
  (if (<= n 1) n (list '+ (fib-exp (- n 2)) (fib-exp (- n 1)))))
```

终端实测：

```
scm> (fib 2)
1
scm> (fib 6)
8
scm> (fib-exp 6)
(+ (+ (+ 0 1) (+ 1 (+ 0 1))) (+ (+ 1 (+ 0 1)) (+ (+ 0 1) (+ 1 (+ 0 1)))))
scm> (fib-exp 4)
(+ (+ 0 1) (+ 1 (+ 0 1)))
scm> (fib-exp 5)
(+ (+ 1 (+ 0 1)) (+ (+ 0 1) (+ 1 (+ 0 1))))
scm> (eval (fib-exp 6))
8
```

---

# Generating Code

### Quasiquotation

有两种给 expression 加引号的方式：

```scheme
Quote:      '(a b)  =>  (a b)
Quasiquote: `(a b)  =>  (a b)
```

它们的区别在于，quasiquote 里的部分内容可以用 `,` **unquote（解除引用）**：

```scheme
(define b 4)

Quote:      '(a ,(+ b 1))  =>  (a (unquote (+ b 1)))
Quasiquote: `(a ,(+ b 1))  =>  (a 5)
```

**` 反引号后面可以用 `,` 来代表不会直接作为字符不动的东西（即会被求值再插入）。**

Quasiquotation 特别适合用来生成 Scheme 表达式：

```scheme
(define (make-add-procedure n) `(lambda (d) (+ d ,n)))

(make-add-procedure 2)  => (lambda (d) (+ d 2))
```

## Example: While Statements

用 quasiquotation 把一个 Python 风格的 `while` 循环翻译成一个可以被 `eval` 的 Scheme 表达式。

**问题一**：小于 10 的偶数（从 2 开始）的平方和？

Python：

```python
x = 2
total = 0
while x < 10:
    total = total + x * x
    x = x + 2
```

对应 Scheme：

```scheme
(begin
  (define (f x total)
    (if (< x 10)
        (f (+ x 2) (+ total (* x x)))
        total))
  (f 2 0))
```

**问题二**：平方小于 50 的数（从 1 开始）之和？

Python：

```python
x = 1
total = 0
while x * x < 50:
    total = total + x
    x = x + 1
```

对应 Scheme：

```scheme
(begin
  (define (f x total)
    (if (< (* x x) 50)
        (f (+ x 1) (+ total x))
        total))
  (f 1 0))
```

## sum-while：用 quasiquotation 生成上面这类循环

```scheme
(define (sum-while initial-x condition add-to-total update-x)
  ; (sum-while 1 '(< (* x x) 50) 'x '(+ x 1))
  `(begin
     (define (f x total)
       (if ,condition
           (f ,update-x (+ total ,add-to-total))
           total))
     (f ,initial-x 0)))
```

终端实测：

```
scm> (define result (sum-while 1 '(< (* x x) 50) 'x '(+ x 1)))
result
scm> result
(begin (define (f x total) (if (< (* x x) 50) (f (+ x 1) (+ total x)) total)) (f 1 0))
scm> (list? result)
#t
scm> (car result)
begin
scm> (eval result)
28
scm> (eval (sum-while 2 '(< x 10) '(* x x) '(+ x 2)))
120
```

**`define result` 为后面的式子；这是个 list，可以 `eval` 得出结果，`car` 是 `begin`。**

---

## Related

- [[Scheme]]
- [[Macros]]
- [[Interpreters]]

---

# 截图

![[截屏2026-08-01 11.27.53.png]]

![[截屏2026-08-01 11.30.09.png]]

![[截屏2026-08-01 11.31.28.png]]

![[截屏2026-08-01 11.59.33.png]]

![[截屏2026-08-01 12.01.56.png]]

![[截屏2026-08-01 12.02.56.png]]
