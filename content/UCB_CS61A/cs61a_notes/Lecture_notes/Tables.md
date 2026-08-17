---
tags:
  - cs61a
  - sql
  - tables
  - aggregation
---

# Joining Tables

终端示例（`dogs`、`parents` 两个表的隐式 join）：

```sql
sqlite> SELECT * FROM dogs, parents WHERE name=child;
```

| name | fur | parent | child |
|---|---|---|---|
| ace | long | finn | ace |
| bella | short | ace | bella |
| charlie | long | ace | charlie |
| daisy | long | finn | daisy |
| finn | curly | ellie | finn |
| ginger | short | finn | ginger |
| hank | curly | daisy | hank |

```sql
sqlite> SELECT * FROM dogs, parents WHERE fur=child;
sqlite> SELECT * FROM dogs, parents WHERE name=parent;
```

| name | fur | parent | child |
|---|---|---|---|
| ace | long | ace | bella |
| ace | long | ace | charlie |
| daisy | long | daisy | hank |
| ellie | short | ellie | finn |
| finn | curly | finn | ace |
| finn | curly | finn | daisy |
| finn | curly | finn | ginger |

```sql
sqlite> SELECT * FROM dogs, parents WHERE name=child AND fur="curly";
```

| name | fur | parent | child |
|---|---|---|---|
| finn | curly | ellie | finn |
| hank | curly | daisy | hank |

```sql
sqlite> SELECT parent FROM dogs, parents WHERE name=child AND fur="curly";
```

| parent |
|---|
| ellie |
| daisy |

## Implicit & Explicit Join Syntax

A join typically has some conditions for matching up the rows of two (or more) tables.

```sql
SELECT parent FROM parents, dogs WHERE child = name
    AND fur = "curly";
```

（`child = name` 这个条件把 `parents` 里的一行和 `dogs` 里的一行匹配起来）

- **Implicit syntax**：用逗号（或直接 `JOIN`），把所有条件都放进 `WHERE` 子句
- **Explicit syntax**：用 `FROM ___ JOIN ___ ON ___`，把匹配条件放在 `ON` 后面

```sql
SELECT parent FROM parents JOIN dogs ON child = name
    WHERE fur = "curly";
```

Explicit syntax is now far more common, but you'll find plenty of both forms in old code.

---

# Aliases & Dot Expressions

### Joining a Table with Itself

Two tables may share a column name; dot expressions and aliases disambiguate column values。

```sql
SELECT [columns] FROM [table] WHERE [condition] ORDER BY [order];
```

`[table]` is a comma-separated list of table names with optional aliases.

Select all pairs of siblings：

```sql
SELECT a.child AS first, b.child AS second
  FROM parents AS a, parents AS b
  WHERE a.parent = b.parent AND a.child < b.child;
```

| first | second |
|---|---|
| bella | charlie |
| ace | daisy |
| ace | ginger |
| daisy | ginger |

（右侧配图是一个层级关系图：`E → F`，`F` 下有三个分支 `A`、`D`、`G`，`A` 下有 `B`、`C`，`D` 下有 `H`，用来示意树状家族关系里"自连接"能找出的同层节点。）

终端示例：

```sql
sqlite> SELECT * FROM parents AS a, parents AS b WHERE a.parent = b.parent AND a.child <> b.child;
```

| parent | child | parent | child |
|---|---|---|---|
| ace | bella | ace | charlie |
| ace | charlie | ace | bella |
| finn | ace | finn | daisy |
| finn | ace | finn | ginger |
| finn | daisy | finn | ace |
| finn | daisy | finn | ginger |
| finn | ginger | finn | ace |
| finn | ginger | finn | daisy |

```sql
sqlite> SELECT * FROM parents AS a, parents AS b WHERE a.parent = b.parent AND a.child < b.child;
```

| parent | child | parent | child |
|---|---|---|---|
| ace | bella | ace | charlie |
| finn | ace | finn | daisy |
| finn | ace | finn | ginger |
| finn | daisy | finn | ginger |

```sql
sqlite> SELECT child FROM parents AS a, parents AS b WHERE a.parent = b.parent AND a.child < b.child;
Parse error: ambiguous column name: child
  SELECT child FROM parents AS a, parents AS b WHERE a.parent = b.parent AND a.c
         ^--- error here
sqlite> SELECT a.child, b.child FROM parents AS a, parents AS b WHERE a.parent = b.parent AND a.child < b.child;
```

| child | child |
|---|---|
| bella | charlie |
| ace | daisy |
| ace | ginger |
| daisy | ginger |

### Joining Multiple Tables

Multiple tables can be joined to yield all combinations of rows from each。

```sql
CREATE TABLE grandparents AS
  SELECT a.parent AS grandog, b.child AS granpup
    FROM parents AS a, parents AS b
    WHERE b.parent = a.child;
```

Select all grandparents with the same fur as their grandchildren. Which tables need to be joined together?

```sql
SELECT grandog FROM grandparents, dogs AS c, dogs AS d
    WHERE grandog = c.name AND
          granpup = d.name AND
          c.fur = d.fur;
```

（配图是同一张层级关系图 `E → F → {A, D, G}`，`A → {B, C}`，`D → H`，用来示意 grandparent/grandchild 关系需要跨三张表 join。）

---

# Numerical Expressions

Expressions can contain function calls and arithmetic operators：

```sql
[expression] AS [name], [expression] AS [name], ...
SELECT [columns] FROM [table] WHERE [expression] ORDER BY [expression];
```

- **Combine values**：`+`, `-`, `*`, `/`, `%`, `and`, `or`
- **Transform values**：`abs`, `round`, `not`, `-`
- **Compare values**：`<`, `<=`, `>`, `>=`, `<>`, `!=`, `=`

有两个不等于，一个 `<>` 和 `!=`；等于只要一个 `=`。

终端示例（`ex.sql`）：

```sql
create table cities as
  select 38 as latitude, 122 as longitude, "Berkeley" as name union
  select 42,              71,              "Cambridge"       union
  select 45,              93,              "Minneapolis"     union
  select 33,              117,             "San Diego"       union
  select 26,              80,              "Miami"           union
  select 90,              0,               "North Pole";

create table cold as
  select name from cities where latitude >= 43;

create table distances as
  select a.name as first, b.name as second,
         60*(b.latitude - a.latitude) as distance
    from cities as a, cities as b;
```

```
~/lec$ sqlite3 -init ex.sql
-- Loading resources from ex.sql

SQLite version 3.8.4.3 2014-04-03 16:53:12
Enter ".help" for usage hints.
sqlite> select second from distances
   ...>         where first = "Minneapolis"
   ...>         order by distance;
Miami
San Diego
Berkeley
Cambridge
Minneapolis
North Pole
sqlite>
```

这个例子好好学习！**`create table …… as` 这是在创建新 table，`from …… as a，…… as b`；逗号分号别忘！**

---

# String Expressions

String values can be combined to form longer strings：

```sql
sqlite> select "hello," || " world";
hello, world
```

Basic string manipulation is built into SQL, but differs from Python：

```sql
sqlite> create table phrase as select "hello, world" as s;
sqlite> select substr(s, 4, 2) || substr(s, instr(s, " ")+1, 1) from phrase;
low
```

Strings can be used to represent structured values, but doing so is rarely a good idea：

```sql
sqlite> create table lists as select "one" as car, "two,three,four" as cdr;
sqlite> select substr(cdr, 1, instr(cdr, ",")-1) as cadr from lists;
two
```

用 string 去表示 data structure 是不好的！（就是最后一种情况）

终端示例（用自连接生成所有"X and Y chased A and B"句子）：

```sql
create table nouns as
  select "dog" as phrase union
  select "cat"           union
  select "bird";

create table ands as
  select first.phrase || " and " || second.phrase as phrase
    from nouns as first, nouns as second
    where first.phrase <> second.phrase;

select subject.phrase || " chased " || object.phrase
  from ands as subject, ands as object
  where subject.phrase <> object.phrase;
```

输出（节选）：

```
bird and dog chased bird and cat
bird and dog chased cat and bird
bird and dog chased cat and dog
bird and dog chased dog and bird
bird and dog chased dog and cat
cat and bird chased bird and cat
cat and bird chased bird and dog
cat and bird chased cat and dog
cat and bird chased dog and bird
cat and bird chased dog and cat
cat and dog chased bird and cat
cat and dog chased bird and dog
cat and dog chased cat and bird
cat and dog chased dog and bird
cat and dog chased dog and cat
dog and bird chased bird and cat
dog and bird chased bird and dog
dog and bird chased cat and bird
dog and bird chased cat and dog
dog and bird chased dog and cat
dog and cat chased bird and cat
dog and cat chased bird and dog
dog and cat chased cat and bird
dog and cat chased cat and dog
dog and cat chased dog and bird
```

## Related

- [[SQL]]
- [[Databases]]
- [[Aggregation]]

---

# 截图

![[截屏2026-08-08 11.39.27.png]]

![[截屏2026-08-08 11.38.57.png]]

![[截屏2026-08-08 11.55.58.png]]

![[截屏2026-08-08 11.56.59.png]]

![[截屏2026-08-08 11.57.11.png]]

![[截屏2026-08-08 12.05.01.png]]

![[截屏2026-08-08 12.02.52.png]]

![[截屏2026-08-09 11.34.34.png]]

![[截屏2026-08-09 11.35.21.png]]
