---
course: cs61A
---
  ----
  
   
![[截屏2026-06-22 12.54.40.png]]
![[截屏2026-06-22 12.58.16.png]]
# Function Currying
**Currying**: Transforming a multi-argument function into a single-argument, higher-order function.
把一个接受多个参数的函数，转换成一系列每次只接受一个参数的函数。
```python
def make_adder(n):
	return lambda k: n+k
	
m = make_adder(2)
print(m(3))
print(m(5))
```---