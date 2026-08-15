---
course: cs61A
---
2026.7.4
# Processing container values

![[截屏2026-07-04 20.57.30.png]]


# Strings

```python
'add_func = lambda x: lambda y: x+y'
exec('add_func = lambda x: lambda y: x+y') #exec能让里面的跑起来！
a = add_func(3)(4)
print(a)
```


# DIctionaries

### Dictionary comprehensions

```python
dic = {x*x:x for x in [1,2,3,4,5] if x > 2}
print(dic)
```