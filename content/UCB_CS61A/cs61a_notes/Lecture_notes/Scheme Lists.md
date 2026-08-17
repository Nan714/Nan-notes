---
tags:
  - cs61a
  - scheme
  - lists
---

# Lists

在 1950 年代，计算机科学家用了一些容易让人困惑的名字：

- **cons**：接受两个参数、创建一个 linked list 的过程
- **car**：返回一个 list 第一个元素的过程
- **cdr**：返回一个 list 除第一个元素外剩余部分的过程
- **nil**：空 list

**cdr 读作 ku:der，像 Python 中的 linked list。**

重要！Scheme list 用括号包裹，元素之间用空格分隔：

```scheme
> (cons 1 (cons 2 nil))
(1 2)
> (define x (cons 1 (cons 2 nil)))
> x
(1 2)
> (car x)
1
> (cdr x)
(2)
> (cons 1 (cons 2 (cons 3 (cons 4 nil))))
(1 2 3 4)
```

`(cons 2 nil)` 画成盒子图就是 `[2 | nil]`；`(cons 1 (cons 2 nil))` 就是 `[1 | •]→[2 | nil]`（一节一节地往后指）。

**一定是后面先 cons，再后面一起和前面 cons，要不然就会出现 `((1 2) 3)` 这种情况。**

终端实测（用 `draw` 画出 list 的盒子指针结构）：

```scheme
scm> (define s (cons 1 (cons 2 nil)))
s
scm> s
(1 2)
scm> (draw s)

  •──▶[1|•]──▶[2|()]

scm> (cons 3 s)
(3 1 2)
scm> (cons 4 (cons 3 s))
(4 3 1 2)
```

```scheme
scm> (draw (cons 4 (cons 3 s)))

  •──▶[4|•]──▶[3|•]──▶[1|•]──▶[2|()]

scm> (cons (cons 4 (cons 3 nil)) s)
((4 3) 1 2)
scm> (draw (cons (cons 4 (cons 3 nil)) s))

  •──▶[•|•]──▶[1|•]──▶[2|()]
       │
       ▼
      [4|•]──▶[3|()]
```

`(cons (cons 4 (cons 3 nil)) s)` 的第一个元素本身又是一个 list（`(4 3)`），所以盒子图里第一个格子指向另外一条链，这样就得到嵌套 list `((4 3) 1 2)`。

```scheme
scm> (car (car (cons (cons 4 (cons 3 nil)) s)))
4
scm> s
(1 2)
scm> (cons s (cons s nil))
((1 2) (1 2))
scm> (draw (cons s (cons s nil)))

  •──▶[•|•]──▶[•|()]
       │       │
       ▼       ▼
      [1|•]──▶[2|()]  (两个格子都指向同一条 (1 2) 链)
```

`list?` 与 `null?`：

```scheme
scm> s
(1 2)
scm> (list? s)
#t
scm> (list? 3)
#f
scm> (list? (car s))
#f
scm> (car s)
1
scm> (list? nil)
#t
```

```scheme
scm> (null? nil)
#t
scm> (null? s)
#f
scm> (list 1 2 3 4)
(1 2 3 4)
scm> (draw (list 1 2 3 4))

  •──▶[1|•]──▶[2|•]──▶[3|•]──▶[4|()]

scm> (cdr (list 1 2 3 4))
(2 3 4)
```

---

# Symbolic Programming

Symbol 通常指向一个值；那要怎么直接指代 symbol 本身呢？

```scheme
> (define a 1)
> (define b 2)
> (list a b)
(1 2)
```

结果值里完全看不出 "a" 和 "b"。

Quotation（引用）用来在 Lisp 里直接指代 symbol 本身：

```scheme
> (list 'a 'b)
(a b)
> (list 'a b)
(a 2)
```

`'a` 是 `(quote a)` 的简写：一种特殊形式，表示"表达式本身就是值"。

Quotation 也可以作用在 combination 上，用来构造 list：

```scheme
> '(a b c)
(a b c)
> (car '(a b c))
a
> (cdr '(a b c))
(b c)
```

**quotation 只有前引号没有后引号；用 quotation 就是直接 refers to symbols；创建 list 也可以直接用 quotation 创建。**

终端实测：

```scheme
scm> 'a
a
scm> (quote a)
a
scm> (cons 'a nil)
(a)
scm> (cons (quote a) nil)
(a)
scm> '(1 2)
(1 2)
scm> '(1 a)
(1 a)
```

```scheme
scm> (list 1 'a)
(1 a)
scm> (list 1 a)
Traceback (most recent call last):
  0    (list 1 a)
  1    a
Error: unknown identifier: a
scm> '(1 (2 3) 4)
(1 (2 3) 4)
scm> (car (cdr (car (cdr '(1 (2 3) 4)))))
3
```

（`(list 1 a)` 报错是因为这里的 `a` 没有被 quote，Scheme 会把它当成一个未定义的变量来求值。）

---

# List processing

内置的 list 处理过程：

- **`(append s t)`**：把 `s` 和 `t` 的元素列在一起；`append` 可以作用于超过 2 个 list
- **`(map f s)`**：把过程 `f` 作用在 list `s` 的每个元素上，把结果列成一个 list
- **`(filter f s)`**：把过程 `f` 作用在 list `s` 的每个元素上，把结果为真的那些元素列成一个 list
- **`(apply f s)`**：把过程 `f` 作用在 list `s` 的元素上，将这些元素作为参数调用 `f`

终端实测：

```scheme
scm> (define s (cons 1 (cons 2 nil)))
s
scm> s
(1 2)
scm> (append s s)
(1 2 1 2)
scm> (append s s s s)
(1 2 1 2 1 2 1 2)
scm> (list s s s s)
((1 2) (1 2) (1 2) (1 2))
scm> (map even? s)
(#f #t)
scm> (map (lambda (x) (* 2 x)) s)
(2 4)
scm> (filter even? s)
(2)
scm> (filter even? '(5 6 7 8 9))
(6 8)
scm> (filter list? '(5 (6 7) 8 (9)))
((6 7) (9))
scm> (map (lambda (s) (cons 5 s)) (filter list? '(5 (6 7) 8 (9))))
((5 6 7) (5 9))
scm> (apply quotient '(10 5))
2
scm> (apply + '(1 2 3 4))
10
scm> (+ 1 2 3 4)
10
scm> (map + '(1 2 3 4))
(1 2 3 4)
scm> (list (+ 1) (+ 2) (+ 3) (+ 4))
(1 2 3 4)
```

---

# Example：Even Subsets

**定义**：一个 list `s` 的 *non-empty subset*（非空子集），是一个包含 `s` 中部分元素的 list。（非空子集可以包含 `s` 的全部元素，但不能一个都不包含。）

```scheme
;;; Non-empty subsets of integer list s that have an even sum
;;;
;;; scm> (even-subsets '(3 4 5 7))
;;; ((5 7) (4 5 7) (4) (3 7) (3 5) (3 4 7) (3 4 5))
(define (even-subsets s) ...)
```

一种递归的思路：`s` 的 even subsets 包括……

- `s` 剩余部分（rest of `s`）的所有 even subsets
- `s` 的第一个元素，后面接上剩余部分的一个（even/odd）subset
- 如果第一个元素本身是偶数，那么仅由它自己组成的 subset

代码的写法逻辑是根据前一页 slide，三种情况：

```scheme
;;; non-empty subsets of integer list s that have an even sum
(define (even-subsets s)
  (if (null? s) nil
      (append (even-subsets (cdr s))
              (map (lambda (t) (cons (car s) t))
                   (if (even? (car s))
                       (even-subsets (cdr s))
                       (odd-subsets (cdr s))))
              (if (even? (car s)) (list (list (car s))) nil))))

;;; non-empty subsets of integer list s that have an odd sum
(define (odd-subsets s)
  (if (null? s) nil
      (append (odd-subsets (cdr s))
              (map (lambda (t) (cons (car s) t))
                   (if (odd? (car s))
                       (even-subsets (cdr s))
                       (odd-subsets (cdr s))))
              (if (odd? (car s)) (list (list (car s))) nil))))
```

终端实测：

```scheme
~/lec $ ./scheme -i ex.scm
Welcome to the CS 61A Scheme Interpreter (version 1.2.5)

scm> (even-subsets '(3 4 5 7))
((5 7) (4 5 7) (4) (3 7) (3 5) (3 4 7) (3 4 5))
scm> (even-subsets '(3 4 5 7 8))
((8) (5 7 8) (5 7) (4 8) (4 5 7 8) (4 5 7) (4) (3 7 8) (3 7) (3 5 8) (3 5) (3 4 7 8) (3 4 7) (3 4 5 8) (3 4 5))
scm> (odd-subsets '(3 4 5 7 8))
((7 8) (7) (5 8) (5) (4 7 8) (4 7) (4 5 8) (4 5) (3 8) (3) (3 7 8) (3 5 7) (3 4 8) (3 4 5 7 8) (3 4 5 7) (3 4) (3))
scm> (map (lambda (s) (apply + s)) (odd-subsets '(3 4 5 7 8)))
(15 7 13 5 19 11 17 9 11 23 15 15 27 19 7 3)
```

可以看出 even 和 odd 重复性很大，可以考虑来一个 sub-helper。用 `subset-helper` 重构后：

```scheme
;;; non-empty subsets of integer list s that have an even sum
(define (even-subsets s)
  (if (null? s) nil
      (append (even-subsets (cdr s))
              (subset-helper even? s))))

;;; non-empty subsets of integer list s that have an odd sum
(define (odd-subsets s)
  (if (null? s) nil
      (append (odd-subsets (cdr s))
              (subset-helper odd? s))))

(define (subset-helper f s)
  (append
   (map (lambda (t) (cons (car s) t))
        (if (f (car s))
            (even-subsets (cdr s))
            (odd-subsets (cdr s))))
   (if (f (car s)) (list (list (car s))) nil)))
```

---

## Related

- [[Scheme]]
- [[Trees]]
- [[Programming as data]]

---

# 截图

![[截屏2026-07-26 15.15.01.png]]

![[截屏2026-07-26 15.16.44.png]]

![[截屏2026-07-26 15.17.02.png]]

![[截屏2026-07-26 15.17.42.png]]

![[截屏2026-07-26 15.18.37.png]]

![[截屏2026-07-26 15.19.48.png]]

![[截屏2026-07-26 15.28.39.png]]

![[截屏2026-07-26 15.31.13.png]]

![[截屏2026-07-26 15.33.47.png]]

![[截屏2026-07-26 15.53.31.png]]

![[截屏2026-07-27 15.40.29.png]]

![[截屏2026-07-28 16.52.38.png]]

![[截屏2026-07-28 16.53.08.png]]

![[截屏2026-07-28 16.54.11.png]]
