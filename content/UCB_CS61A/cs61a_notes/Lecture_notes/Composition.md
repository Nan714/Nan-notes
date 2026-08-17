---
tags:
  - cs61a
  - python
  - functions
  - composition
---

# Linked lists

## Linked List Structure

一个 linked list（链表）要么是空的，要么是一个 first value 加上剩下部分的 linked list。

- 一个 linked list 是一个 pair（对）
- 第一个（第零个）元素是一个 attribute value，存在 `first` 里
- 剩下的元素被存储在一个 linked list 里，存在 `rest` 里
- 一个 class attribute `Link.empty` 代表一个空的 linked list

```
Link(3, Link(4, Link(5, Link.empty)))
```

## Linked List Class

链表类：属性通过 `__init__` 传入。

```python
class Link:
    empty = ()  # 某种长度为 0 的序列

    def __init__(self, first, rest=empty):
        assert rest is Link.empty or isinstance(rest, Link)
        self.first = first
        self.rest = rest
```

`help(isinstance)`：Return whether an object is an instance of a class or of a subclass thereof.

```python
Link(3, Link(4, Link(5)))
```

终端例子：

```python
>>> Link(3, Link(4, Link(5)))
Link(3, Link(4, Link(5)))
>>> s = Link(3, Link(4, Link(5)))
>>> s.first
3
>>> s.rest
Link(4, Link(5))
>>> s.rest.first
4
>>> s.rest.rest.first
5
>>> s.rest.rest.rest is Link.empty
True
>>> s.rest.first = 7
>>> s
Link(3, Link(7, Link(5)))
>>> Link(8, s.rest)
Link(8, Link(7, Link(5)))
>>> s
Link(3, Link(7, Link(5)))
```

It's not usual to mutate（修改）the link, but you can create new link.

**The last 2 expressions can see that it won't change the original Link.**

---

# Linked List Processing

```python
def range_link(start, end):
    """Return a Link containing consecutive integers from start to end.

    >>> range_link(3, 6)
    Link(3, Link(4, Link(5)))
    """
    if start >= end:
        return Link.empty
    else:
        return Link(start, range_link(start + 1, end))


def map_link(f, s):
    """Return a Link that contains f(x) for each x in Link s.

    >>> map_link(square, range_link(3, 6))
    Link(9, Link(16, Link(25)))
    """
    if s is Link.empty:
        return s
    else:
        return Link(f(s.first), map_link(f, s.rest))


def filter_link(f, s):
    """Return a Link that contains only the elements x of Link s for which f(x)
    is a true value.

    >>> filter_link(odd, range_link(3, 6))
    Link(3, Link(5))
    """
    if s is Link.empty:
        return s
    filtered_rest = filter_link(f, s.rest)
    if f(s.first):
        return Link(s.first, filtered_rest)
    else:
        return filtered_rest
```

终端例子：

```python
~/lec$ python3 -m doctest ex.py
~/lec$ python3 -i ex.py
>>> r = range_link(1, 6)
>>> s = filter_link(odd, r)
>>> t = map_link(square, s)
>>> t
Link(1, Link(9, Link(25)))
```

---

# Linked list mutation

## Linked Lists Can Change

属性赋值语句可以改变一个 Link 的 `first` 和 `rest` 属性。

链表的 `rest` 可以包含这个链表自身作为子链表（形成环）。

```python
>>> s = Link(1, Link(2, Link(3)))
>>> s.first = 5
>>> t = s.rest
>>> t.rest = s
>>> s.first
5
>>> s.rest.rest.rest.rest.rest.first
2
```

Global frame 中 `s` 指向 first=5 的节点，`t` 指向 first=2 的节点；`t.rest = s` 让 first=2 的节点的 `rest` 又指回 first=5 的节点，形成一个环。（Note: 实际的 environment diagram 会更复杂。）

s.rest is t, t.rest is s, so the answer would only be 5 or 2. Based on the number of "rest", the answer is 2.

---

# Tree Class

一个 Tree 有一个 `label` 和一个 branches 的 list；每个 branch 也是一个 Tree。

```python
class Tree:
    def __init__(self, label, branches=[]):
        self.label = label
        for branch in branches:
            assert isinstance(branch, Tree)
        self.branches = list(branches)


def fib_tree(n):
    if n == 0 or n == 1:
        return Tree(n)
    else:
        left = fib_tree(n - 2)
        right = fib_tree(n - 1)
        fib_n = left.label + right.label
        return Tree(fib_n, [left, right])
```

用函数式（非 class）风格实现同样的结构：

```python
def tree(label, branches=[]):
    for branch in branches:
        assert is_tree(branch)
    return [label] + list(branches)

def label(tree):
    return tree[0]

def branches(tree):
    return tree[1:]

def fib_tree(n):
    if n == 0 or n == 1:
        return tree(n)
    else:
        left = fib_tree(n - 2)
        right = fib_tree(n - 1)
        fib_n = label(left) + label(right)
        return tree(fib_n, [left, right])
```

终端例子（带有 `indented`、`is_leaf`、`leaves`、`height` 等辅助方法）：

```python
~/lec$ python3 -i ex.py
>>> print(fib_tree(6))
8
  3
    1
      0
      1
    2
      1
      1
        0
        1
  5
    2
      1
      1
        0
        1
    3
      1
        0
        1
      2
        1
        1
          0
          1
>>> height(fib_tree(6))
5
```

```python
def indented(self):
    lines = []
    for b in self.branches:
        for line in b.indented():
            lines.append('  ' + line)
    return [str(self.label)] + lines

def is_leaf(self):
    return not self.branches


def fib_tree(n):
    """A Fibonacci tree."""
    if n == 0 or n == 1:
        return Tree(n)
    else:
        left = fib_tree(n - 2)
        right = fib_tree(n - 1)
        fib_n = left.label + right.label
        return Tree(fib_n, [left, right])


def leaves(t):
    """Return a list of leaf labels in Tree T."""
    if t.is_leaf():
        return [t.label]
    else:
        all_leaves = []
        for b in t.branches:
            all_leaves.extend(leaves(b))
        return all_leaves


def height(t):
    """Return the number of transitions in the longest path in T."""
    if t.is_leaf():
        return 0
    else:
        return 1 + max([height(b) for b in t.branches])
```

---

# Tree Mutation

## Example: Pruning Trees

从一棵树上删除子树被称为 **pruning（剪枝）**。剪枝要在递归处理之前进行。

给定一棵树，根为 `3`，两个分支分别为标签 `1`（叶子 `0`、`1`）和标签 `2`（分支为叶子 `1`，以及标签 `1` 下的叶子 `0`、`1`）：

```python
def prune(t, n):
    """Prune all sub-trees whose label is n."""
    t.branches = [b for b in t.branches if b.label != n]
    for b in t.branches:
        prune(b, n)
```

## Related

- [[Functions]]
- [[Decomposition]]
- [[Higher-ordered functions]]

---

# 截图

![[截屏2026-07-11 10.17.16.png]]

![[截屏2026-07-21 14.25.47.png]]

![[截屏2026-07-21 14.27.12.png]]

![[截屏2026-07-21 14.41.03.png]]

![[截屏2026-07-21 14.48.55.png]]

![[截屏2026-07-21 14.54.11.png]]

![[截屏2026-07-21 14.54.53.png]]

![[截屏2026-07-21 14.57.50.png]]
