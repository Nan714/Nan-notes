---
tags:
  - cs61a
  - data-abstraction
  - python
---

# Lists

**Lists in Environment Diagrams**

Assume that before each example below we execute:

```python
s = [2, 3]
t = [5, 6]
```

| Operation | Example | Result |
|---|---|---|
| **append** adds one element to a list | `s.append(t)`<br>`t = 0` | `s → [2, 3, [5, 6]]`<br>`t → 0` |
| **extend** adds all elements in one list to another list | `s.extend(t)`<br>`t[1] = 0` | `s → [2, 3, 5, 6]`<br>`t → [5, 0]` |
| **addition** & **slicing** create new lists containing existing elements | `a = s + [t]`<br>`b = a[1:]`<br>`a[1] = 9`<br>`b[1][1] = 0` | `s → [2, 3]`<br>`t → [5, 0]`<br>`a → [2, 9, [5, 0]]`<br>`b → [3, [5, 0]]` |
| The **list** function also creates a new list containing existing elements | `t = list(s)`<br>`s[1] = 0` | `s → [2, 0]`<br>`t → [2, 3]` |
| **slice assignment** replaces a slice with new values | `s[0:0] = t`<br>`s[3:] = t`<br>`t[1] = 0` | `s → [5, 6, 2, 5, 6]`<br>`t → [5, 0]` |

| Operation | Example | Result |
|---|---|---|
| **pop** removes & returns the last element | `t = s.pop()` | `s → [2]`<br>`t → 3` |
| **remove** removes the first element equal to the argument | `t.extend(t)`<br>`t.remove(5)` | `s → [2, 3]`<br>`t → [6, 5, 6]` |

---

# Objects

**Land Owners**

Instance attributes are found before class attributes; class attributes are inherited

```python
class Worker:
    greeting = 'Sir'
    def __init__(self):
        self.elf = Worker
    def work(self):
        return self.greeting + ', I work'
    def __repr__(self):
        return Bourgeoisie.greeting

class Bourgeoisie(Worker):
    greeting = 'Peon'
    def work(self):
        print(Worker.work(self))
        return 'I gather wealth'

jack = Worker()
john = Bourgeoisie()
jack.greeting = 'Maam'
```

```
>>> Worker().work()
'Sir, I work'

>>> jack
Peon

>>> jack.work()
'Maam, I work'

>>> john.work()
Peon, I work
'I gather wealth'

>>> john.elf.work(john)
'Peon, I work'
```

对应对象图：
- `<class Worker>`：`greeting: 'Sir'`
- `<class Bourgeoisie>`：`greeting: 'Peon'`
- `jack <Worker>`：`elf: → <class Worker>`，`greeting: 'Maam'`
- `john <Bourgeoisie>`：`elf: → <class Worker>`

---

# Iterables and Iterators

```python
def min_abs_indices(s):
    """Indices of all elements in list s that have
    the smallest absolute value.

    >>> min_abs_indices([-4, -3, -2, 3, 2, 4])
    [2, 4]
    >>> min_abs_indices([1, 2, 3, 4, 5])
    [0]
    """
    min_abs = min(map(abs, s))
    return [i for i in range(len(s)) if abs(s[i]) == min_abs]

def largest_adj_sum(s):
    """Largest sum of two adjacent elements in a list s.

    >>> largest_adj_sum([-4, -3, -2, 3, 2, 4])
    6
    >>> largest_adj_sum([-4, 3, -2, -3, 2, -4])
    1
    """
    return max([s[i] + s[i+1] for i in range(len(s) - 1)])
```

```python
def digit_dict(s):
    """Map each digit d to the lists of elements in s that
    end with d.

    >>> digit_dict([5, 8, 13, 21, 34, 55, 89])
    {1: [21], 3: [13], 4: [34], 5: [5, 55], 8: [8], 9: [89]}
    """
    return {d: [x for x in s if x % 10 == d] for d in range(10)
            if any([x % 10 == d for x in s])}

def all_have_an_equal(s):
    """Does every element equal some other element in s?

    >>> all_have_an_equal([-4, -3, -2, 3, 2, 4])
    False
    >>> all_have_an_equal([4, 3, 2, 3, 2, 4])
    True
    """
    return all([s[i] in s[:i]+s[i+1:] for i in range(len(s))])
```

#built-in_function

- `any`：有一个是 True，结果就是 True
- `all`：全部都对才是 True

什么是 False？
- `False`
- `None`
- `0`
- `0.0`
- `''`
- `[]`
- `()`
- `{}`
- `set()`

## Related

- [[Data Abstraction]]
- [[Trees]]
- [[Containers]]

---

# 截图

![[截屏2026-07-22 11.22.10.png]]

![[截屏2026-07-22 11.22.53.png]]

![[截屏2026-07-22 11.43.44.png]]

![[截屏2026-07-22 15.13.26.png]]

![[截屏2026-07-22 15.13.54.png]]
