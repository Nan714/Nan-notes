---
tags:
  - cs61a
  - python
  - generators
  - iterators
---

# Generators and Generator Functions

**Generator（生成器）是一种特殊的 Iterator（迭代器）。**
**所有 generator 都是 iterator，但不是所有 iterator 都是 generator。**

```
Iterator（迭代器）
│
├── list_iterator
├── tuple_iterator
├── dict_keyiterator
├── ...
└── Generator（生成器）
```

**Iterator 是一种对象，它可以一个一个地产生数据。** 它必须实现两个方法：

- `__iter__()`
- `__next__()`

`a = next(b)` 这个 `a` 是一个行动（往后移一个的行动）。

Generator 是 **Python 自动帮你写好的 iterator**。

```python
>>> def plus_minus(x):
...     yield x
...     yield -x

>>> t = plus_minus(3)
>>> next(t)
3
>>> next(t)
-3
>>> t
<generator object plus_minus ...>
```

- A **generator function** is a function that **yield**s values instead of **return**ing them
- A normal function **return**s once; a *generator function* can **yield** multiple times
- A *generator* is an iterator created automatically by calling a *generator function*
- When a *generator function* is called, it returns a *generator* that iterates over its yields

**list 收集最外层 generator 产生的值，也就是说过程中 yield 只返回上级，不会出现在 list 中。**

### yield from

```python
def countdown(k):
    if k > 0:
        yield k
        # yield countdown(k-1) 这样是不行的！因为这就返回了一个 generator 而不是数值

        # for x in countdown(k-1):
        #     yield x
        # 以上可以写为
        yield from countdown(k-1)
    else:
        yield 'Blast off'
```

`prefixes` 和 `substrings` 也是用 `yield from` 递归实现的例子：

```python
def prefixes(s):
    if s:
        yield from prefixes(s[:-1])
        yield s

def substrings(s):
    if s:
        yield from prefixes(s)
        yield from substrings(s[1:])
```

终端演示：

```
~/lec$ python3 -i ex.py
>>> prefixes('both')
<generator object prefixes at 0x102379f30>
>>> list(prefixes('both'))
['b', 'bo', 'bot', 'both']
>>> ^D
~/lec$ python3 -i ex.py
>>> substrings('tops')
<generator object substrings at 0x101379f30>
>>> list(substrings('tops'))
['t', 'to', 'top', 'tops', 'o', 'op', 'ops', 'p', 'ps', 's']
>>>
```

---

# Example: partitions

```python
def partitions(n, m):
    """Yield partitions.

    >>> for p in partitions(6, 4): print(p)
    2 + 4
    1 + 1 + 4
    3 + 3
    1 + 2 + 3
    1 + 1 + 1 + 3
    2 + 2 + 2
    1 + 1 + 2 + 2
    1 + 1 + 1 + 1 + 2
    1 + 1 + 1 + 1 + 1 + 1
    """
    if n > 0 and m > 0:
        if n == m:
            yield str(m)
        for p in partitions(n - m, m):
            yield p + ' + ' + str(m)
        yield from partitions(n, m - 1)
```

终端演示：

```
~/lec $ python3 -i ex.py
>>> s = list(partition(60, 50))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'partition' is not defined
>>> s = list(partitions(60, 50))
>>> len(s)
966370
>>> next(partitions(60, 50))
'10 + 50'
>>> t = partitions(60, 50)
>>> for _ in range(10):
...     print(next(t))
...
10 + 50
1 + 9 + 50
2 + 8 + 50
1 + 1 + 8 + 50
3 + 7 + 50
1 + 2 + 7 + 50
1 + 1 + 1 + 7 + 50
4 + 6 + 50
1 + 3 + 6 + 50
2 + 2 + 6 + 50
```

hw05 中的 `yield_paths`：

```python
'''hw 05'''
if label(t) == value:
    yield [value]

for b in branches(t):
    for path in yield_paths(b, value):
        yield [label(t)] + path

# 不能直接 yield [label(t)] + yield_paths(b, value)! 因为后面这个是 generator，不能用 list ➕ generator！
# 我们要相加的是 generator 得出来的结果，所以要 for path in yield_paths
```

## Related

- [[Iterators]]
- [[Sequence]]
- [[Containers]]

---

# 截图

![[截屏2026-07-06 16.23.16.png]]

![[截屏2026-07-06 18.31.06.png]]

![[截屏2026-07-06 18.38.53.png]]
