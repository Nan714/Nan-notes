---
tags:
  - cs61a
  - data-abstraction
  - abstraction
---

2026.07.05

# Abstraction Barriers

**Violating Abstraction Barriers**

```python
add_rational([1, 2], [1, 4])   # Does not use constructors, and uses the list literal twice!

def divide_rational(x, y):
    return [x[0] * y[1], x[1] * y[0]]   # No selectors! And no constructor!
```

- `add_rational([1, 2], [1, 4])` 直接用 list 字面量而不是通过 constructor 来创建 rational number，违反了抽象屏障
- `divide_rational` 的实现直接用下标 `x[0]`、`x[1]`、`y[0]`、`y[1]` 取值，而不是用 selector（如 `numer`、`denom`），也没有用 constructor 包装返回值，同样违反了抽象屏障

# Data Representation

**Rational Data Abstraction Implemented as Functions**

```python
def rational(n, d):
    def select(name):
        if name == 'n':
            return n
        elif name == 'd':
            return d
    return select

def numer(x):
    return x('n')

def denom(x):
    return x('d')
```

- `rational(n, d)` 这个 constructor 返回的是一个函数 `select`（Constructor is a higher-order function）
- `select` 这个内部函数代表了一个 rational number（This function represents a rational number）
- `numer`、`denom` 这两个 selector 通过调用这个对象本身来取值（Selector calls the object itself）

示例：

```python
x = rational(3, 8)
numer(x)
```

环境图：
- Global frame：`rational`、`numer`、`denom` → 对应函数；`x` → `func select(name) [parent=f1]`
- `f1: rational [parent=Global]`：`n = 3`，`d = 8`，`select` → `func select`，`Return value` → `select`
- `f2: numer [parent=Global]`：`x` → `select`，`Return value = 3`
- `f3: select [parent=f1]`：`name = "n"`，`Return value = 3`

## Related

- [[Data Example]]
- [[Trees]]
- [[Representation]]

---

# 截图

![[截屏2026-07-05 12.49.10.png]]

![[截屏2026-07-05 12.37.31.png]]
