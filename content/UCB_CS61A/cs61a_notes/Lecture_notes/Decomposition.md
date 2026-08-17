---
tags:
  - cs61a
  - functions
  - decomposition
---

# Modular Design

**A design principle**: Isolate different parts of the program that address different concerns

A modular component can be developed and tested independently

# Example

```python
def search(query, ranking=lambda r: -r.stars):
    results = [r for r in Restaurant.all if query(r)]
    return sorted(results, key=ranking)

def reviewed_both(r, s):
    return fast_overlap(r.reviewers, s.reviewers)

def fast_overlap(s, t):
    count, i, j = 0, 0, 0
    while i < len(s) and j < len(t):
        if s[i] == t[j]:
            count, i, j = count + 1, i + 1, j + 1
        elif s[i] < t[j]:
            i += 1
        else:
            j += 1
    return count

class Restaurant:
    all = []
    def __init__(self, name, stars, reviewers):
        self.name, self.stars = name, stars
        self.reviewers = reviewers
        Restaurant.all.append(self)

    def similar(self, k, similarity=reviewed_both):
        "Return the K most similar restaurants to SELF."
        others = Restaurant.all
        others.remove(self)
        different = lambda r: -similarity(r, self)
        return sorted(others, key=different)[:k]

    def __repr__(self):
        return '<' + self.name + '>'
```

数据加载与交互部分：

```python
import json

reviewers_for_restaurant = {}
for line in open('reviews.json'):
    r = json.loads(line)
    biz = r['business_id']
    if biz not in reviewers_for_restaurant:
        reviewers_for_restaurant[biz] = [r['user_id']]
    else:
        reviewers_for_restaurant[biz].append(r['user_id'])

for line in open('restaurants.json'):
    r = json.loads(line)
    reviewers = reviewers_for_restaurant[r['business_id']]
    Restaurant(r['name'], r['stars'], sorted(reviewers))

while True:
    print('>', end=' ')
    results = search(input().strip())
    for r in results:
        print(r, 'shares reviewers with', r.similar(3))
```

这个例子把程序拆成了几个独立、各司其职的模块：`search`（排序 & 查询）、`fast_overlap`/`reviewed_both`（相似度计算）、`Restaurant` 类（数据表示）、以及数据加载 & 主交互循环，彼此之间通过清晰的接口（函数签名、类方法）耦合。

---

# Set Intersection

**Linear-Time Intersection of Sorted Lists**

Given two sorted lists with no repeats, return the number of elements that appear in both.

```python
def fast_overlap(s, t):
    """Return the overlap between sorted S and sorted T.

    >>> fast_overlap([3, 4, 6, 7, 9, 10], [1, 3, 5, 7, 8])
    2
    """
    i, j, count = 0, 0, 0
    while i < len(s) and j < len(t):
        if s[i] == t[j]:
            count, i, j = count + 1, i + 1, j + 1
        elif s[i] < t[j]:
            i = i + 1
        else:
            j = j + 1
    return count
```

用两个指针 `i`、`j` 分别在两个有序列表 `s`、`t` 上从左往右移动：
- 若两指针指向的元素相等，计数加一，两个指针都前进
- 若 `s[i] < t[j]`，说明 `s[i]` 不可能出现在 `t` 的剩余部分中，`i` 前进
- 否则 `j` 前进

这样只需遍历一遍即可求出交集大小，时间复杂度是线性的（对应 `Decomposition` 中 `fast_overlap` 的实现）。

## Related

- [[Functions]]
- [[Composition]]
- [[Recursion]]

---

# 截图

![[截屏2026-07-15 15.55.53.png]]

![[截屏2026-07-15 16.02.10.png]]

![[截屏2026-08-12 13.32.13.png]]
