---
tags:
  - cs61a
  - sql
  - aggregation
  - databases
---

# Aggregation

## Aggregate Functions

到目前为止，所有 SQL 表达式一次只引用单独一行里的值。**aggregate function（聚合函数）** 在 `[columns]` 子句中，会从一组行里计算出一个值。

```sql
select [columns] from [table] where [expression] order by [expression];
```

示例表 `animals`：

```sql
create table animals as
  select "dog"     as kind, 4 as legs, 20 as weight union
  select "cat"            , 4        , 10           union
  select "ferret"          , 4        , 10           union
  select "parrot"          , 2        , 6            union
  select "penguin"         , 2        , 10           union
  select "t-rex"           , 2        , 12000;
```

| kind | legs | weight |
|---|---|---|
| dog | 4 | 20 |
| cat | 4 | 10 |
| ferret | 4 | 10 |
| parrot | 2 | 6 |
| penguin | 2 | 10 |
| t-rex | 2 | 12000 |

```sql
select max(legs) from animals;
```

| max(legs) |
|---|
| 4 |

在 sqlite 终端里的实际例子：

```
sqlite> select max(legs-weight) + 5 from animals;
1
sqlite> select max(legs), min(weight) from animals;
4|6
sqlite> select max(legs) - min(weight) from animals;
-2
sqlite> select min(legs), max(weight) from animals
   ...>   where name <> "t-rex";
Error: no such column: name
sqlite> select min(legs), max(weight) from animals
   ...>   where kind <> "t-rex";
2|20

sqlite> select avg(legs) from animals;
3.0
sqlite> select count(legs) from animals;
6
sqlite> select count(kind) from animals;
6
sqlite> select count(weight) from animals;
6
sqlite> select count(*) from animals;
6
sqlite> select count(distinct legs) from animals;
2
sqlite> select count(distinct weight) from animals;
4
sqlite> select sum(distinct weight) from animals;
12036
```

**max min sum avg count arithmetic**（常见的聚合函数：最大值、最小值、求和、平均值、计数，也可以做算术运算）

同一个 `select` 语句里可以把 aggregate function 和普通 column 混在一起，但要注意结果中非聚合的 column 的值来自哪一行是不确定的：

```
sqlite> select max(weight) from animals;
12000
sqlite> select max(weight), kind from animals;
12000|t-rex
sqlite> select min(weight), kind from animals;
6|parrot
sqlite> select min(kind), kind from animals;
cat|cat
sqlite> select min(kind), legs, weight from animals;
cat|4|10
sqlite> select avg(weight) from animals;
2009.33333333333
sqlite> select avg(weight), kind from animals;
2009.33333333333|t-rex
sqlite> select max(legs) from animals;
4
sqlite> select max(legs), kind from animals;
4|cat
```

也可以多个结合在一起！

---

# Grouping

### Grouping rows

表里的行可以被分组（group），聚合会在每一组内分别进行。

```sql
select [columns] from [table] group by [expression] having [expression];
```

**分组的数量 = 某个表达式的不同取值个数。**

```sql
select legs, max(weight) from animals group by legs;
```

`animals` 表按 `legs` 分组：`legs=4` 组包含 dog/cat/ferret，`legs=2` 组包含 parrot/penguin/t-rex。

| legs | max(weight) |
|---|---|
| 4 | 20 |
| 2 | 12000 |

用 group by

在 sqlite 终端里的实际例子：

```
sqlite> select legs from animals group by legs;
2
4
sqlite> select legs,count(*) from animals group by legs;
2|3
4|3
sqlite> select legs, max(weight) from animals group
        by legs;
2|12000
4|20
sqlite> select legs, weight from animals
   ...>   group by legs, weight;
2|6
2|10
2|12000
4|10
4|20
sqlite> select max(kind), weight/legs
   ...>   from animals
   ...>   group by weight/legs;
ferret|2
parrot|3
penguin|5
t-rex|6000
```

SQL 中 integer 相除就是整数除法，所以 10/4 是 2

### Selecting groups

`having` 子句用于过滤参与聚合的分组集合。

```sql
select [columns] from [table] group by [expression] having [expression];
```

```sql
select weight/legs, count(*) from animals group by weight/legs having count(*)>1;
```

`animals` 表按 `weight/legs` 分组后（值分别为 5, 2, 2, 3, 5, 6000），只保留出现次数大于 1 的分组：

| weight/legs | count(*) |
|---|---|
| 5 | 2 |
| 2 | 2 |

用 having 去 select

---

## Related

- [[SQL]]
- [[Tables]]
- [[Databases]]

---

# 截图

![[截屏2026-08-09 11.42.34.png]]

![[截屏2026-08-09 11.50.31.png]]

![[截屏2026-08-09 11.50.49.png]]

![[截屏2026-08-09 11.51.39.png]]

![[截屏2026-08-09 13.04.08.png]]

![[截屏2026-08-09 13.06.38.png]]

![[截屏2026-08-09 13.07.19.png]]
