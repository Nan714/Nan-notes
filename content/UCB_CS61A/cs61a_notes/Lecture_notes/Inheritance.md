---
tags:
  - cs61a
  - oop
  - inheritance
---

# Object-Oriented Design

## Designing for Inheritance

- Don't repeat yourself; use existing implementations.
- Attributes that have been overridden are still accessible via class objects.
- Look up attributes on instances whenever possible.

**Use existing implementations**

```python
class CheckingAccount(Account):
    """A bank account that charges for withdrawals."""
    withdraw_fee = 1
    interest = 0.01

    def withdraw(self, amount):
        return Account.withdraw(self, amount + self.withdraw_fee)
```

- `Account.withdraw`：Attribute look-up on base class
- `self.withdraw_fee`：Preferred to `CheckingAccount.withdraw_fee` to allow for specialized accounts（用 `self.withdraw_fee` 而不是写死类名，是为了让更下层的子类也能覆盖这个属性）

## Related

- [[Object-Oriented Programming]]
- [[Attributes]]
- [[Representation]]

---

# 截图

![[截屏2026-07-07 14.06.02.png]]
