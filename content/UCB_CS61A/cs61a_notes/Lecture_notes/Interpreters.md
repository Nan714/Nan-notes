# interpreting scheme

![[截屏2026-07-31 14.03.22.png]]

---

# Special Forms

![[截屏2026-07-31 14.15.38.png]]

---

# Logical forms

![[截屏2026-07-31 14.25.00.png]]

![[截屏2026-07-31 14.25.18.png]]

**这个判断和python的T/F and or很像。如果if 跟着true，就直接走true那边，就算alternative那边有error，也不会报错，因为就不会走那边**

---

# Quotation
evaluate to…… 求值后得到…… or 计算结果是……

![[截屏2026-07-31 14.30.14.png]]
**The scheme_read parser converts shorthand to a combination.**

---

# Lambda expressions


![[截屏2026-07-31 14.34.29.png]]

![[截屏2026-07-31 14.37.41.png]]

![[截屏2026-07-31 14.37.55.png]]

define 就是定义，lookup就是查找。
这个frame和python一样意思

---

# Define expressions

![[截屏2026-07-31 14.58.54.png]]
**倒数两行 这两行完全equivalent**

---

# Dynamic scope

![[截屏2026-07-31 15.07.46.png]]

lexical scope就是在哪里defined的
dynamic scope就是在哪里被called的