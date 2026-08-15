# Databases

![[截屏2026-08-04 13.41.58.png]]

![[截屏2026-08-04 13.43.15.png]]

---

# Structured query language

type sqlite3 in the terminal

![[截屏2026-08-06 11.23.23.png]]

![[截屏2026-08-06 11.23.38.png]]

正常来说 不会用到那么多的union，因为一般我们是用已有的表格去创建新表格，但这里是我们从头开始创建一个新表格，所以有很多的union

---

# Projecting tables

![[截屏2026-08-06 11.28.36.png]]

**最后的；别忘了！然后parent不带引号指的是column，带引号“ace”指的是具体的值**

![[截屏2026-08-06 11.30.59.png]]
**.mode column 会让输出更规整 有个column的样子**

---

# Arithmetic

![[截屏2026-08-06 11.38.52.png]]

![[截屏2026-08-06 11.39.14.png]]

![[截屏2026-08-06 11.39.54.png]]

可以看到这个顺序和create的时候的顺序并不一样，这是declarative的特性，它会按自己的算法来，可以看出这里是按首字母的顺序来的

第二题：
```sql
select word from ints
where one +two/2 + four/4 + eight/8 = 1;
```