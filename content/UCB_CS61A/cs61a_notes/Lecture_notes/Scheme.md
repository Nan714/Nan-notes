Scheme Specification: https://insideempire.github.io/CS61A-Website-Archive/articles/scheme-spec/index.html#let
lisp language

Let: (let ([binding] ...) <body> ...)


# Scheme

```scheme
(+ 1 2)
(* 2 2 2 3 3)
(* (/ (- (+ 2 6) 2) 3) 4)
```

![[截屏2026-07-24 14.44.34.png]]

---
# Special Forms

![[截屏2026-07-24 17.44.50.png]]![[截屏2026-07-24 17.57.31.png]]

![[截屏2026-07-24 18.00.40.png]]

**这里有定义函数，嵌套函数，if语句判断，recursion（因为update又被call了）
update 1的意思是从1开始找
update的方法是牛顿法**
\text{new guess} = \frac{\text{guess}+\frac{x}{guess}}{2}
也就是
average(
    guess,
    x / guess
)

---

# lambda expressions

lambda (x) 这里x的括号一定要加！！！

![[截屏2026-07-26 13.55.26.png]]

---
# More Special Forms

### Cond & Begin

![[截屏2026-07-26 14.09.11.png]]

**if 适用于两种条件，cond适用于大于等于三种条件
begin后面可以接多个表达，所以需要用begin**

---

# Example: Sierpinski's triangle

![[截屏2026-07-26 14.32.56.png]]

里面是recursion