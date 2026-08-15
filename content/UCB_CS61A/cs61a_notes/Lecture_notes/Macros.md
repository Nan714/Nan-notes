lst = (a,b,c)
想要取到c
car (cdr (cdr lst))
外层的car不能忘！不加car的话取出来的就是(c)，是个list，加上car就是c，是这个元素本身

# Expressions

![[截屏2026-08-01 14.57.36.png]]
反引号中可以用逗号来进行运算

---

# Macros

![[截屏2026-08-03 11.32.01.png]]
这个就是先不evaluate 先call，然后最后再evaluate

![[截屏2026-08-03 11.36.11.png]]
**define**

	如果是define的话 后面的expr如果不括号括起来的话，就会先evaluate，导致print 2变为None；
	所以define 需要将expr括起来，但这个时候依旧不会运算；
	想要运算的话，用eval

**define-macro**

	define-macro就会直接运算， macro其实就是scheme里定义各种函数的东西，让内置函数跑起来
	用list（）是在造代码写出来代码，而不是直接（）运行代码


---
# For Macro


![[截屏2026-08-03 12.00.34.png]]
看到list了吗！要用起来！
lambda 后面跟的symbol一定是一个list！

---

# Trace

![[截屏2026-08-03 15.18.10.png]]
左边的是decorator，
```python
@decorator
def greet():
    print("Hello!")
    
# 这两个是等价的！

def greet():
    print("Hello!")

greet = decorator(greet)
```

**其实就是fact变成了wrapper （traced），通过greet = decorator(greet)（提示： return traced）可以看出**

右边scheme的逻辑就是先给出fact函数，再让original成为fact，然后再重新定义fact。所以original就是原来的fact，fact就是新定义的。
![[截屏2026-08-03 15.50.55.png]]

![[截屏2026-08-03 15.24.59.png]]
如果是(quote ,operator) 那么就会直接给出fact，因为从最开始的define（红色）来看，operator是（car expr）就是fact


