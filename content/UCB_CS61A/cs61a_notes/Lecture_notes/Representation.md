# String representations
![[截屏2026-07-08 15.14.54.png]]


![[截屏2026-07-08 15.15.31.png]]


- **`repr(obj)`**：给程序员看的，追求**准确**，最好能够还原对象。
- **`str(obj)`**：给用户看的，追求**易读**。

```python
s = "HEllo\nWorld"
print(s)
print()
print(str(s))
print()
print(repr(s))
```
---
# String interpolation

f-strings
![[截屏2026-07-08 15.25.38.png]]

![[截屏2026-07-08 15.26.10.png]]

---

# polymorphic functions

**同一个函数可以作用于多种不同类型的数据，而不需要为每种类型单独写一个版本。**

“Poly” 表示**多**，“morphic” 表示**形式**，所以 polymorphic 就是**多种形式**。

	eg. len()就是一个polymorphic function
	同一个 `len()`：
	- 可以用于字符串
	- 可以用于列表
	- 可以用于元组
	- 可以用于字典

![[截屏2026-07-08 15.45.34.png]]
Only Class attributes can implement repr

### interfaces （接口）


![[截屏2026-07-08 15.46.57.png]]

![[截屏2026-07-08 15.44.57.png]]

---
# Special Method Names in Python

![[截屏2026-07-08 16.16.58.png]]


![[截屏2026-07-08 16.17.22.png]]
