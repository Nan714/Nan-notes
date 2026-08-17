---
tags:
  - cs61a
  - scheme
---

Scheme Specification: `https://insideempire.github.io/CS61A-Website-Archive/articles/scheme-spec/index.html#let`

lisp language

`Let: (let ([binding] ...) <body> ...)`

# Scheme

```scheme
(+ 1 2)
(* 2 2 2 3 3)
(* (/ (- (+ 2 6) 2) 3) 4)
```

终端实测（数字与谓词函数）：

```scheme
scm> (*)
1
scm> (* 2 2 2 2 2 3 3 3}
Traceback (most recent call last):
Error: invalid numeral or symbol: 3}
scm> (* 2 2 2 2 2 3 3 3)
864
scm> (- (* 2 2 2 2 2 3 3 3) 1)
863
scm> (/ (+ (* (- (* 2 2 2 2 2 3 3 3) 1) 7) 1) 3)
2014
scm> +
#[+]
scm> (number? 3)
#t
scm> (number? +)
#f
scm> (zero? 2)
#f
scm> (zero? 0)
#t
scm> (zero? (- 2 2))
#t
scm> (integer? 2.2)
#f
scm> (integer? 2)
#t
```

（第二行 `(* 2 2 2 2 2 3 3 3}` 用了错误的括号 `}` 收尾，导致 "invalid numeral or symbol" 报错；改用 `)` 收尾后正常求值。）

---

# Special Forms

一个不是 call expression 的 combination 就是一个 **special form**：

- **If** expression：`(if <predicate> <consequent> <alternative>)`
- **And** and **or**：`(and <e1> ... <en>)`，`(or <e1> ... <en>)`
- **Binding symbols**：`(define <symbol> <expression>)`
- **New procedures**：`(define (<symbol> <formal parameters>) <body>)`

**Evaluation**（对 `if`）：
1. 求值 predicate 表达式。
2. 根据结果求值 consequent 或 alternative 二者之一。

```scheme
> (define pi 3.14)
> (* pi 2)
6.28
```

符号 "pi" 在 global frame 中被绑定为 3.14。

```scheme
> (define (abs x)
    (if (< x 0)
        (- x)
        x))
> (abs -3)
3
```

一个过程被创建并绑定到符号 "abs"。

终端实测（`square` / `average`）：

```scheme
scm> (define (square x) (* x x))
square
scm> (square 16)
256
scm> (define (average x y)
       (/ (+ x y) 2))
average
scm> (average 3 7)
5
```

牛顿法求平方根（`sqrt` / 内部嵌套定义的 `update`）：

```scheme
scm> (define (sqrt x)
       (define (update guess)
         (if (= (square guess) x)
             guess
             (update (average guess (/ x guess)))))
       (update 1))
sqrt
scm> (sqrt 256)
16
```

**这里有定义函数，嵌套函数，if 语句判断，recursion（因为 update 又被 call 了）。
update 1 的意思是从 1 开始找。
update 的方法是牛顿法：**

$$\text{new guess} = \frac{\text{guess}+\frac{x}{guess}}{2}$$

也就是

```scheme
(average
  guess
  (/ x guess))
```

---

# lambda expressions

**`lambda (x)` 这里 x 的括号一定要加！！！**

Lambda expression 求值得到一个匿名过程：

```scheme
(lambda (<formal-parameters>) <body>)
```

两个等价的表达式：

```scheme
(define (plus4 x) (+ x 4))

(define plus4 (lambda (x) (+ x 4)))
```

operator 本身也可以是一个 call expression：

```scheme
((lambda (x y z) (+ x y (square z))) 1 2 3)
```

这会求值为 *add-x-&-y-&-z²* 这个过程。

---

# More Special Forms

### Cond & Begin

`cond` special form 的行为类似 Python 里的 `if-elif-else`：

```python
if x > 10:
    print('big')
elif x > 5:
    print('medium')
else:
    print('small')
```

```scheme
(cond ((> x 10) (print 'big))
      ((> x 5)  (print 'medium))
      (else     (print 'small)))
```

也可以整个 `cond` 表达式作为 `print` 的参数：

```scheme
(print
  (cond ((> x 10) 'big)
        ((> x 5)  'medium)
        (else     'small)))
```

`begin` special form 把多个表达式合并成一个表达式：

```python
if x > 10:
    print('big')
    print('guy')
else:
    print('small')
    print('fry')
```

```scheme
(cond ((> x 10) (begin (print 'big)   (print 'guy)))
      (else     (begin (print 'small) (print 'fry))))

(if (> x 10) (begin
               (print 'big)
               (print 'guy))
             (begin
               (print 'small)
               (print 'fry)))
```

**if 适用于两种条件，cond 适用于大于等于三种条件；begin 后面可以接多个表达，所以需要用 begin。**

---

# Example: Sierpinski's triangle

用 turtle 画谢尔宾斯基三角形：

```scheme
(define (line) (fd 50))
(define (twice fn) (fn) (fn))
(define (repeat k fn)
  (fn)
  (if (> k 1) (repeat (- k 1) fn)))
(define (tri fn)
  (repeat 3 (lambda () (fn) (lt 120))))
(define (sier d k)
  (tri (lambda () (if (= d 1) (fd k) (leg d k)))))
(define (leg d k)
  (sier (- d 1) (/ k 2))
  (penup) (fd k) (pendown))
```

终端实测：

```
~/lec$ ./scheme
Welcome to the CS 61A Scheme Interpreter (version 1.2.4)
scm> (rt 90)
scm> (speed 0)
scm> (sier 5 200)
Traceback (most recent call last):
  0    (sier 5 200)
  1    sier
Error: unknown identifier: sier
scm> (load 'ex.scm)

scm> (sier 5 200)
scm>
```

（第一次调用 `sier` 报错是因为还没有 `load` 定义它的文件；`load` 之后再调用就能画出图形。）

里面是 recursion：`sier` 递归调用 `leg`，`leg` 又递归调用 `sier`，每次深度 `d` 减 1，直到 `d = 1` 时画一条边。

---

## Related

- [[Scheme Lists]]
- [[Interpreters]]
- [[Macros]]
- [[Calculator]]

---

# 截图

![[截屏2026-07-24 14.44.34.png]]

![[截屏2026-07-24 17.44.50.png]]

![[截屏2026-07-24 17.57.31.png]]

![[截屏2026-07-24 18.00.40.png]]

![[截屏2026-07-26 13.55.26.png]]

![[截屏2026-07-26 14.09.11.png]]

![[截屏2026-07-26 14.32.56.png]]
