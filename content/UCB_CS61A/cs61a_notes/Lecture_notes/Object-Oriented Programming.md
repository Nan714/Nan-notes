---
tags:
  - cs61a
  - oop
---

# OOP

## Object-Oriented Programming

A method for organizing programs:
- Extends data abstraction
- Bundles together information and related behavior

A metaphor for computation using distributed state:
- Each **object** has its own local state
- Each object also knows how to manage its own local state
- Interact with an object using its **methods**
- Several objects may all be instances of a common **class**
- Different classes may relate to each other

Specialized syntax & vocabulary to support this metaphor:
- A **class** defines how objects of a particular type behave
- An **object** is an instance of a class; the class is its type
- A **method** is a function called on an object using a dot expression: `s.append(5)`

# Class statement

## The Account Class

```python
class Account:
    def __init__(self, account_holder):
        self.balance = 0
        self.holder = account_holder

    def deposit(self, amount):
        self.balance = self.balance + amount
        return self.balance

    def withdraw(self, amount):
        if amount > self.balance:
            return 'Insufficient funds'
        self.balance = self.balance - amount
        return self.balance
```

- `__init__` is a special method name for the function that constructs an Account instance
- `self` is the instance of the Account class on which `deposit` was invoked: `a.deposit(10)`
- Methods are functions defined in a class statement

```python
>>> a = Account('John')
>>> a.holder
'John'
>>> a.balance
0
>>> a.deposit(15)
15
>>> a.withdraw(10)
5
>>> a.balance
5
>>> a.withdraw(10)
'Insufficient funds'
```

---
# Creating instances

## Instance Attributes

An object's attributes are accessed and modified using dot expressions:

```python
>>> a = Account('Alan')
>>> a.balance
0
>>> a.balance = 12
>>> a.balance
12
```

The value of an existing attribute can be changed.

Any attribute can be assigned any value:

```python
>>> b = Account('Ada')
>>> b.balance
0
>>> b.balance = 20
>>> a.backup = b
>>> a.backup.balance
20
```

**A new attribute can be added at any time**

## Object Identity

Every object that is an instance of a user-defined class has a unique identity:

```python
>>> a = Account('John')
>>> b = Account('Jack')
>>> a.balance
0
>>> b.holder
'Jack'
```

Every call to `Account` creates a new Account instance. There is only one Account class.

Identity operators "is" and "is not" test if two expressions evaluate to the same object:

```python
>>> a is a
True
>>> a is not b
True
```

Binding an object to a new name using assignment does not create a new object:

```python
>>> c = a
>>> c is a
True
```

---

## Related

- [[Attributes]]
- [[Inheritance]]
- [[Representation]]

---

# 截图

![[截屏2026-07-06 18.43.12.png]]

![[截屏2026-07-06 18.46.41.png]]

![[截屏2026-07-06 18.52.28.png]]

![[截屏2026-07-06 18.49.57.png]]
