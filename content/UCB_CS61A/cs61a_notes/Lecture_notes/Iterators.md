---
tags:
  - cs61a
  - python
  - iterators
  - sequences
---

# Iterators

## Iterators

A container can provide an iterator that provides access to its elements in some order.

- `iter(iterable)`: Return an iterator over the elements of an iterable value
- `next(iterator)`: Return the next element in an iterator

```python
>>> s = [3, 4, 5]
>>> t = iter(s)
>>> next(t)
3
>>> next(t)
4

>>> u = iter(s)
>>> next(u)
3
>>> next(t)
5
>>> next(u)
4
```

```python
s = [[1,2],3,4,5]
t = iter(s)
print(next(t))
print(next(t))
print(list(t)) # list 把剩下的全部打印出来
```

---
# Dictionary iteration

## Views of a Dictionary

- An *iterable* value is any value that can be passed to **iter** to produce an iterator.
- An *iterator* is returned from **iter** and can be passed to **next**; all iterators are mutable.
- A dictionary, its keys, its values, and its items are all iterable values.
  - The order of items in a dictionary is the order in which they were added (Python 3.6+)
  - Historically, items appeared in an arbitrary order (Python 3.5 and earlier)

```python
>>> d = {'one': 1, 'two': 2, 'three': 3}
>>> d['zero'] = 0
>>> k = iter(d.keys())   # or iter(d)
>>> next(k)
'one'
>>> next(k)
'two'
>>> next(k)
'three'
>>> next(k)
'zero'

>>> v = iter(d.values())
>>> next(v)
1
>>> next(v)
2
>>> next(v)
3
>>> next(v)
0

>>> i = iter(d.items())
>>> next(i)
('one', 1)
>>> next(i)
('two', 2)
>>> next(i)
('three', 3)
>>> next(i)
('zero', 0)
```

修改字典大小时再迭代会报错：

```python
>>> d = {'one': 1, 'two': 2}
>>> k = iter(d)
>>> next(k)
'one'
>>> d['zero'] = 0
>>> next(k)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
RuntimeError: dictionary changed size during iteration
>>> d
{'one': 1, 'two': 2, 'zero': 0}
>>> k = iter(d)
>>> next(k)
'one'
>>> next(k)
'two'
>>> d['zero'] = 5
>>> next(k)
'zero'
```

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

## Built-in Functions for Iteration

Many built-in Python sequence operations return iterators that compute results lazily.

- `map(func, iterable)`: Iterate over `func(x)` for x in iterable
- `filter(func, iterable)`: Iterate over x in iterable if `func(x)`
- `zip(first_iter, second_iter)`: Iterate over co-indexed (x, y) pairs
- `reversed(sequence)`: Iterate over x in a sequence in reverse order

To view the contents of an iterator, place the resulting elements into a container:

- `list(iterable)`: Create a list containing all x in iterable
- `tuple(iterable)`: Create a tuple containing all x in iterable
- `sorted(iterable)`: Create a sorted list containing x in iterable

```python
~/lec$ python3 -i ex.py

def double(x):
    print('**', x, '=>', 2*x, '**')
    return 2*x

>>> m = map(double, range(3, 7))
>>> f = lambda y: y >= 10
>>> t = filter(f, m)
>>> next(t)
** 3 => 6 **
** 4 => 8 **
** 5 => 10 **
10
>>> next(t)
** 6 => 12 **
12
>>> list(t)
[]
>>> list(filter(f, map(double, range(3, 7))))
** 3 => 6 **
** 4 => 8 **
** 5 => 10 **
** 6 => 12 **
[10, 12]
```

```python
>>> t = [1, 2, 3, 2, 1]
>>> t
[1, 2, 3, 2, 1]
>>> reversed(t)
<list_reverseiterator object at 0x101b7ad30>
>>> reversed(t) == t
False
>>> list(reversed(t))
[1, 2, 3, 2, 1]
>>> list(reversed(t)) == t
True
```

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

## The Zip Function

The built-in **zip** function returns an iterator over co-indexed tuples.

```python
>>> list(zip([1, 2], [3, 4]))
[(1, 3), (2, 4)]
```

If one iterable is longer than the other, **zip** only iterates over matches and skips extras.

```python
>>> list(zip([1, 2], [3, 4, 5]))
[(1, 3), (2, 4)]
```

More than two iterables can be passed to **zip**.

```python
>>> list(zip([1, 2], [3, 4, 5], [6, 7]))
[(1, 3, 6), (2, 4, 7)]
```

Implement **palindrome**, which returns whether s is the same forward and backward.

```python
>>> palindrome([3, 1, 4, 1, 3])
True
>>> palindrome([3, 1, 4, 1, 5])
False
>>> palindrome('seveneves')
True
>>> palindrome('seven eves')
False
```

终端实现：

```python
~/lec $ python3
>>> s = [1, 2, 3]
>>> list(zip(s, reversed(s)))
[(1, 3), (2, 2), (3, 1)]
>>> s = [3, 1, 4, 1, 3]
>>> list(zip(s, reversed(s)))
[(3, 3), (1, 1), (4, 4), (1, 1), (3, 3)]
>>> ^D
~/lec $ python3 -m doctest ex.py
~/lec $

def palindrome(s):
    """Return whether s is the same backward and forward.

    >>> palindrome([3, 1, 4, 1, 5])
    False
    >>> palindrome([3, 1, 4, 1, 3])
    True
    >>> palindrome('seveneves')
    True
    >>> palindrome('seven eves')
    False
    """
    return all([a == b for a, b in zip(s, reversed(s))])
```

---

## Related

- [[Generators]]
- [[Containers]]
- [[Sequence]]

---

# 截图

![[截屏2026-07-06 13.32.45.png]]

![[截屏2026-07-06 13.35.10.png]]

![[截屏2026-07-06 13.40.29.png]]

![[截屏2026-07-06 15.58.28.png]]

![[截屏2026-07-06 16.04.54.png]]

![[截屏2026-07-06 16.11.10.png]]

![[截屏2026-07-06 16.12.41.png]]

![[截屏2026-07-06 16.13.14.png]]
