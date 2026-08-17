---
tags:
  - cs61a
  - oop
  - attributes
---

# Methods and Functions

Python distinguishes between:

- **Functions**：从课程一开始就在创建的东西
- **Bound methods**：把一个 function 和它要被调用的对象结合在一起

```
Object + Function = Bound Method
```

```python
>>> type(Account.deposit)
<class 'function'>
>>> type(tom_account.deposit)
<class 'method'>

>>> Account.deposit(tom_account, 1001)
1011
>>> tom_account.deposit(1007)
2018
```

# Attribute assignment

## Attribute Assignment Statements

- **Account class attributes**：`interest`（在多次赋值后从 0.02 → 0.04 → 0.05）、以及 `withdraw`、`deposit`、`__init__` 方法
- **Instance attributes of `jim_account`**：`balance: 0`，`holder: 'Jim'`，`interest: 0.08`
- **Instance attributes of `tom_account`**：`balance: 0`，`holder: 'Tom'`（没有自己的 `interest`，所以会向上查找 class attribute）

```python
>>> jim_account = Account('Jim')
>>> tom_account = Account('Tom')
>>> tom_account.interest
0.02
>>> jim_account.interest
0.02
>>> Account.interest = 0.04
>>> tom_account.interest
0.04
>>> jim_account.interest
0.04
```

```python
>>> jim_account.interest = 0.08
>>> jim_account.interest
0.08
>>> tom_account.interest
0.04
>>> Account.interest = 0.05
>>> tom_account.interest
0.05
>>> jim_account.interest
0.08
```

**如果 instance 设立了 attribute，就算 class 变了，instance 的也不改变**

## Related

- [[Object-Oriented Programming]]
- [[Inheritance]]
- [[Representation]]

---

# 截图

![[截屏2026-07-07 13.55.13.png]]

![[截屏2026-07-07 13.57.54.png]]
