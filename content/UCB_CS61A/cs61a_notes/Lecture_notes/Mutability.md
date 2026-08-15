# Objects

![[截屏2026-07-06 10.41.50.png]]

----
# Example: String
### ASCII standard

![[截屏2026-07-06 10.49.45.png]]

### Unicode standard

![[截屏2026-07-06 10.51.21.png]]

```python
from unicodedata import name, lookup
print(name('A'))
print(lookup('SNOWMAN'))
print(lookup('BABY'))
print(lookup('WHITE SMILING FACE'))
```

---
# Tuples
```python
a = (3,[1,2]) # immutable 里面可以有mutable
# 不能 a[0] = 4 修改不了
a[1][0]=3
print(a)
```
---
# Mutation

![[截屏2026-07-06 12.04.31.png]]

---
# Mutable functions

![[截屏2026-07-06 12.08.57.png]]