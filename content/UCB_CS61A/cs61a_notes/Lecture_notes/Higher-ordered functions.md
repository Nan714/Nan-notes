# Iteration Example
### The Fibonacci Sequence

----------

# Control  
### If statements and call expressions
![[截屏2026-01-24 16.53.52.png]]
statements:
- 一点点往下运行
![[截屏2026-01-24 16.56.13.png]]
call expressions：
- 要先把operand运行了再套进去
![[截屏2026-01-24 16.56.46.png]]
因为先运行了sqrt(-16) 所以报错

---------

# Control expressions
### Logical operators

![[截屏2026-01-24 16.59.27.png]]
可以让函数不crash
```python
def has_big_sqrt(x):
	return x>0 and sqrt(x) > 0
```
这样的话就算x是个负数，也不会出现error

-----

# Higher-order functions

![[截屏2026-01-24 18.33.23.png]]
basically the same
**generalization**

summation example
![[截屏2026-01-24 18.38.57.png]]

--------

