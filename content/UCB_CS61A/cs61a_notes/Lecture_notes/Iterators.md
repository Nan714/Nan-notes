# Iterators

![[截屏2026-07-06 13.32.45.png]]

```python
s = [[1,2],3,4,5]
t = iter(s)
print(next(t))
print(next(t))
print(list(t)) # list 把剩下的全部打印出来
```

---
# Dictionary iteration

![[截屏2026-07-06 13.35.10.png]]


![[截屏2026-07-06 13.40.29.png]]
**中途改变字典的大小（增删）是不可以的，但修改可以**

# For statement
```python
r = range(3,6)
ri = iter(r)
next(ri) # 3
for i in ri:
	print(i) #前面有next 所以这里直接从4开始
```

---
# Built-in iterator functions
#built-in_function
#map #filter #zip
![[截屏2026-07-06 15.58.28.png]]

![[截屏2026-07-06 16.04.54.png]]
![[截屏2026-07-06 16.11.10.png]]

```python
nums = [1, 2, 3]

result = map(lambda x: x * 2, nums)

print(list(result))

words = ["cat", "apple", "python"]

result = map(len, words)

print(list(result))
```

---
# Zip
![[截屏2026-07-06 16.12.41.png]]

![[截屏2026-07-06 16.13.14.png]]

