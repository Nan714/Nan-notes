# Databases

## Database Management Systems (DBMS)

- DBMS（数据库管理系统）很重要、被广泛使用，而且很有意思
- **table（表）**：record（记录）的集合，每条 record 是一行（row），每行对每个 column 都有一个值
- **column（列）**：有 name 和 type
- SQL（Structured Query Language）可能是使用最广泛的编程语言
- SQL 是一门 **declarative（声明式）** 编程语言

示例表：

| latitude | longitude | name |
|---|---|---|
| 38 | 122 | Berkeley |
| 42 | 71 | Cambridge |
| 45 | 93 | Minneapolis |

- 一个 **table** 由 columns 和 rows 组成
- 一个 **row** 对每个 column 都有一个值
- 一个 **column** 有 name 和 type

---

# Structured Query Language

在终端输入 `sqlite3` 进入 SQL 交互环境。

## Declarative vs Imperative

- **声明式语言**（如 SQL、Prolog）：
  - 一个"程序"描述的是**想要的结果**
  - interpreter 自己想办法生成结果
- **命令式语言**（如 Python、Scheme）：
  - 一个"程序"描述的是**计算过程**
  - interpreter 按执行/求值规则一步步执行

`cities` 表：

| latitude | longitude | name |
|---|---|---|
| 38 | 122 | Berkeley |
| 41 | 74 | New York |
| 45 | 93 | Minneapolis |
| 34 | 118 | Los Angeles |

```sql
SELECT "West Coast" AS region, name FROM cities
    WHERE longitude >= 115
    ORDER BY latitude;
```

结果：

| region | name |
|---|---|
| West Coast | Los Angeles |
| West Coast | Berkeley |

## Selecting Value Literals

- `SELECT` 语句总是包含一个逗号分隔的 column description 列表
- column description 是一个表达式，后面可选跟 `AS` + 列名：

```sql
SELECT [expression] AS [name], [expression] AS [name];
```

- 单独 select 字面量（literal）会创建一个只有一行的表
- 两个 select 语句的 **union** 是一个包含二者所有行的表

```sql
SELECT "daisy" AS parent, "hank" AS child UNION
SELECT "ace"           , "bella"          UNION
SELECT "ace"           , "charlie"        UNION
SELECT "finn"          , "ace"            UNION
SELECT "finn"          , "dixie"          UNION
SELECT "finn"          , "ginger"         UNION
SELECT "ellie"         , "finn";
```

对应的家族树（ellie → finn → {ace, daisy, ginger}，ace → {bella, charlie}，daisy → hank）

正常来说不会用到那么多 union，因为一般是用已有的表格去创建新表格；但这里是从头开始创建一个新表格，所以有很多 union。

## Naming Tables

- `SELECT` 语句的结果只是**显示**给用户看，并不会被存储
- `CREATE TABLE` 语句把一个 `SELECT` 语句的结果**永久命名并保存**：

```sql
create table [name] as [select statement];

CREATE TABLE parents AS
  SELECT "daisy" AS parent, "hank" AS child UNION
  SELECT "ace"           , "bella"          UNION
  SELECT "ace"           , "charlie"        UNION
  SELECT "finn"          , "ace"            UNION
  SELECT "finn"          , "dixie"          UNION
  SELECT "finn"          , "ginger"         UNION
  SELECT "ellie"         , "finn";
```

`parents` 表：

| parent | child |
|---|---|
| ace | bella |
| ace | charlie |
| daisy | hank |
| finn | ace |
| finn | dixie |
| finn | ginger |
| ellie | finn |

---

# Projecting Tables

- `SELECT` 语句可以用 `FROM` 指定输入表
- 用 `WHERE` 可以选出输入表的一个行子集
- 用 `ORDER BY` 可以对剩下的行声明一个顺序
- column description 决定了每一行输入怎样被投影成一行输出

```sql
SELECT [expression] AS [name], [expression] AS [name], ...;
SELECT [columns] FROM [table] WHERE [condition] ORDER BY [order];

SELECT child FROM parents WHERE parent = "ace";
SELECT parent FROM parents WHERE parent > child;
```

**最后的分号 `;` 别忘了！然后 parent 不带引号指的是 column，带引号 "ace" 指的是具体的值。**

在 sqlite 终端里的实际例子（`.mode column` 会让输出更规整，有个 column 的样子）：

```
sqlite> .mode column
sqlite> SELECT child FROM parents;
child
-------
bella
charlie
hank
finn
ace
daisy
ginger

sqlite> SELECT child, parent FROM parents;
child    parent
-------  ------
bella    ace
charlie  ace
hank     daisy
finn     ellie
ace      finn
daisy    finn
ginger   finn

sqlite> SELECT child, parent FROM parents WHERE parent="ace";
child    parent
-------  ------
bella    ace
charlie  ace
```

---

# Arithmetic

- select 表达式中，column 名会求值为对应行的值
- arithmetic 表达式可以结合行的值和常数

```sql
create table lift as
  select 101 as chair, 2 as single, 2 as couple union
  select 102          , 0          , 3          union
  select 103          , 4          , 1;

select chair, single + 2 * couple as total from lift;
```

| chair | total |
|---|---|
| 101 | 6 |
| 102 | 6 |
| 103 | 6 |

## Discussion Question

给定 `ints` 表，描述了如何用 2 的幂次之和组成各个整数：

```sql
create table ints as
  select "zero"  as word, 0 as one, 0 as two, 0 as four, 0 as eight union
  select "one"          , 1        , 0        , 0        , 0        union
  select "two"          , 0        , 2        , 0        , 0        union
  select "three"        , 1        , 2        , 0        , 0        union
  select "four"         , 0        , 0        , 4        , 0        union
  select "five"         , 1        , 0        , 4        , 0        union
  select "six"          , 0        , 2        , 4        , 0        union
  select "seven"        , 1        , 2        , 4        , 0        union
  select "eight"        , 0        , 0        , 0        , 8        union
  select "nine"         , 1        , 0        , 0        , 8;
```

**(A)** 写一个 select 语句，输出每个整数的 `word` 和 `value` 两列：

```sql
select word, one+two+four+eight as value
from ints;
```

结果（按首字母排序，因为这是 declarative 语言，执行顺序由数据库自己决定）：

```
eight|8
five|5
four|4
nine|9
one|1
seven|7
six|6
three|3
two|2
zero|0
```

可以看到这个顺序和 create 的时候的顺序并不一样，这是 declarative 的特性，它会按自己的算法来，这里是按首字母的顺序。

**(B)** 写一个 select 语句，输出 2 的幂次对应的 `word` 名字：

```sql
select word from ints
where one +two/2 + four/4 + eight/8 = 1;
```

---

# 截图

![[截屏2026-08-04 13.41.58.png]]

![[截屏2026-08-04 13.43.15.png]]

![[截屏2026-08-06 11.23.23.png]]

![[截屏2026-08-06 11.23.38.png]]

![[截屏2026-08-06 11.28.36.png]]

![[截屏2026-08-06 11.30.59.png]]

![[截屏2026-08-06 11.38.52.png]]

![[截屏2026-08-06 11.39.14.png]]

![[截屏2026-08-06 11.39.54.png]]
