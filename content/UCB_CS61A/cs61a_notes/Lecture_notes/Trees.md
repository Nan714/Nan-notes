---
tags:
  - cs61a
  - data-abstraction
  - trees
---

# Trees

## Tree Abstraction

**Recursive description (wooden trees)：**

- A **tree** has a root **label** and a list of **branches**
- Each branch is a **tree**
- A tree with zero branches is called a **leaf**

**Relative description (family trees)：**

- Each location in a tree is called a **node**
- Each **node** has a **label** that can be any value
- One node can be the **parent/child** of another

*People often refer to labels by their locations: "each parent is the sum of its children"*

示例图：根节点 label 为 `3`，有两个分支（branch）。左边分支根 label 为 `1`，它有两个叶子（leaf）分支，label 分别为 `0` 和 `1`。右边分支根 label 为 `2`，它有两个分支：一个叶子 label 为 `1`，另一个子树根 label 为 `1`，其下又有 `0`、`1` 两个叶子。

## Tree Processing

```python
def tree(label, branches=[]):
    for branch in branches:
        assert is_tree(branch), 'branches must be trees'
    return [label] + list(branches)

def label(tree):
    return tree[0]

def branches(tree):
    return tree[1:]

def is_tree(tree):
    if type(tree) != list or len(tree) < 1:
        return False
    for branch in branches(tree):
        if not is_tree(branch):
            return False
    return True

def is_leaf(tree):
    return not branches(tree)

def fib_tree(n):
    if n <= 1:
        return tree(n)
    else:
        left, right = fib_tree(n-2), fib_tree(n-1)
        return tree(label(left)+label(right), [left, right])

def count_leaves(t):
    """Count the leaves of tree T."""
    if is_leaf(t):
        return 1
    else:
        return sum([count_leaves(b) for b in branches(t)])
```

终端示例：

```
~/lec$ python3 -i ex.py
>>> fib_tree(4)
[3, [1, [0], [1]], [2, [1], [1, [0], [1]]]]
>>> count_leaves(fib_tree(4))
5
>>> fib_tree(1)
```

## Printing Trees

```python
def print_tree(t, indent=0):
    print('  ' * indent + str(label(t)))
    for b in branches(t):
        print_tree(b, indent+1)
```

终端示例：

```
~/lec$ python3 -i ex.py
>>> print_tree(fib_tree(4))
3
  1
    0
    1
  2
    1
    1
      0
      1
>>> print_tree(fib_tree(5))
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
>>>
```

## Summing Paths

```python
from tree import *

numbers = tree(3, [tree(4), tree(5, [tree(6)])])

haste = tree('h', [tree('a', [tree('s'),
                               tree('t')]),
                    tree('e')])

def print_sums(t, so_far):
    so_far = so_far + label(t)
    if is_leaf(t):
        print(so_far)
    else:
        for b in branches(t):
            print_sums(b, so_far)
```

终端示例：

```
~/lec$ python3 -i ex.py
>>> print_sums(numbers, 0)
7
14
>>> print_sums(haste, '')
has
hat
he
>>>
```

## Counting Paths

**Count Paths that have a Total Label Sum**

```python
def count_paths(t, total):
    """Return the number of paths from the root to any node in tree t
    for which the labels along the path sum to total.

    >>> t = tree(3, [tree(-1), tree(1, [tree(2, [tree(1)]), tree(3)]), tree(1, [tree(-1)])])
    >>> count_paths(t, 3)
    2
    >>> count_paths(t, 4)
    2
    >>> count_paths(t, 5)
    0
    >>> count_paths(t, 6)
    1
    >>> count_paths(t, 7)
    2
    """
    if label(t) == total:
        found = 1
    else:
        found = 0
    return found + sum([count_paths(b, total - label(t)) for b in branches(t)])
```

示例树（对应 docstring 里的 `t`）：根 label 为 `3`，三个分支 label 分别为 `-1`、`1`、`1`；中间的 `1` 分支下又有 `2`（其下有叶子 `1`）和 `3` 两个分支；最右边的 `1` 分支下有叶子 `-1`。

## Related

- [[Data Abstraction]]
- [[Recursion]]
- [[Scheme Lists]]

---

# 截图

![[截屏2026-07-05 12.55.48.png]]

![[截屏2026-07-06 00.40.31.png]]

![[截屏2026-07-06 00.41.39.png]]

![[截屏2026-07-06 00.42.32.png]]

![[截屏2026-07-06 00.43.31.png]]
