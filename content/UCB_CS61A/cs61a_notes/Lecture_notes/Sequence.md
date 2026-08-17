---
tags:
  - cs61a
  - python
  - sequences
  - control-flow
---

# For Statement

`for` 语句可以对一个序列（sequence）中的元素进行遍历。如果序列里每个元素本身又是一个（长度固定的）序列，可以直接用多个变量名解包（unpack）：

```python
l = [[1,2],[1,1],[2,3],[2,2]]
same = 0
for x, y in l:  # 可以直接用 x，y 指代 list 里的东西
    if x == y:
        same += 1

print(same)
```

如果你不想要 `i`，只想要重复多少次，可用 `_` 代替：

```python
for _ in range(3):
    print("GO")
```

## Related

- [[Containers]]
- [[Iterators]]
- [[Control]]
