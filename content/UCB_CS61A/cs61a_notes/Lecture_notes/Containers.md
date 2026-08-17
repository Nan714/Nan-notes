---
tags:
  - cs61a
  - python
  - containers
  - sequences
---

2026.7.4

# Processing container values

## Sequence Aggregation

一些内置函数接受可迭代对象作为参数，并把它们聚合成一个值：

- `sum(iterable[, start]) -> value`
  返回一个数字可迭代对象（**不是**字符串）的和，加上参数 `start` 的值（默认是 0）。当可迭代对象为空时，返回 `start`。

- `max(iterable[, key=func]) -> value`
  `max(a, b, c, ...[, key=func]) -> value`
  只有单个可迭代对象参数时，返回其中最大的元素；有两个或以上参数时，返回最大的那个参数。

- `all(iterable) -> bool`
  如果可迭代对象中所有的值 `x` 都满足 `bool(x)` 为 `True`，则返回 `True`。如果可迭代对象为空，返回 `True`。

# Strings

```python
'add_func = lambda x: lambda y: x+y'
exec('add_func = lambda x: lambda y: x+y')  # exec 能让里面的跑起来！
a = add_func(3)(4)
print(a)
```

# Dictionaries

### Dictionary comprehensions

```python
dic = {x*x:x for x in [1,2,3,4,5] if x > 2}
print(dic)
```

## Related

- [[Sequence]]
- [[Iterators]]
- [[Mutability]]

---

# 截图

![[截屏2026-07-04 20.57.30.png]]
