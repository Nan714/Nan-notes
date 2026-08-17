---
tags:
  - cs61a
  - sql
  - databases
---

# Create tables and drop tables

## Create Table

`CREATE TABLE` 语法（syntax diagram）：

```
CREATE [TEMP|TEMPORARY] TABLE [IF NOT EXISTS] [schema-name.]table-name
    ( column-def [, column-def]* [, table-constraint]* ) [WITHOUT ROWID]
CREATE [TEMP|TEMPORARY] TABLE [IF NOT EXISTS] [schema-name.]table-name AS select-stmt
```

`column-def`：`column-name [type-name] [column-constraint]*`

`column-constraint`：`[CONSTRAINT name] PRIMARY KEY [ASC|DESC] conflict-clause [AUTOINCREMENT] | NOT NULL conflict-clause | UNIQUE conflict-clause | CHECK (expr) | DEFAULT (signed-number | literal-value | (expr)) | COLLATE collation-name | foreign-key-clause`

## Drop Table

```
DROP TABLE [IF EXISTS] [schema-name.]table-name
```

---

# Modifying tables

## Insert

`INSERT` 语法（syntax diagram，包含 `INSERT`/`REPLACE`/`INSERT OR ROLLBACK`/`INSERT OR ABORT`/`INSERT OR FAIL`/`INSERT OR IGNORE` 等变体）：

```
INSERT INTO table-name (column-name, ...) VALUES (expr, ...) | select-stmt | DEFAULT VALUES
```

For a table `t` with two columns...

要插入到一列：

```sql
INSERT INTO t(column) VALUES (value);
```

要插入到两列：

```sql
INSERT INTO t VALUES (value0, value1);
```

（Demo）终端例子：

```
sqlite> select * from primes;
Error: no such table: primes
sqlite> create table primes(n UNIQUE, prime DEFAULT 1);
sqlite> select * from primes;
sqlite> INSERT INTO primes VALUES (2, 1), (3, 1);
sqlite> select * from primes;
2|1
3|1
sqlite> INSERT INTO primes(n) VALUES (4), (5), (6), (7);
sqlite> select * from primes;
2|1
3|1
4|1
5|1
6|1
7|1
sqlite> INSERT INTO primes(n) SELECT n+6 FROM primes;
sqlite> select * from primes;
2|1
3|1
4|1
5|1
6|1
7|1
8|1
9|1
10|1
11|1
12|1
13|1
sqlite> INSERT INTO primes(n) SELECT n+12 FROM primes;
```

## Update

`UPDATE` 语法（syntax diagram，包含 `UPDATE OR ROLLBACK/ABORT/REPLACE/FAIL/IGNORE`）：

```
UPDATE qualified-table-name SET column-name = expr [, column-name = expr]* [WHERE expr]
```

Update sets all entries in certain columns to new values, just for some subset of rows.

终端例子：

```
sqlite> select * from primes;
2|1
3|1
4|0
5|1
6|0
7|1
8|0
9|1
10|0
11|1
12|0
13|1
14|0
15|1
16|0
17|1
18|0
19|1
20|0
21|1
22|0
23|1
24|0
25|1
sqlite> UPDATE primes SET prime=0 WHERE n>3 AND n%3=0;
sqlite> UPDATE primes SET prime=0 WHERE n>5 AND n%5=0;
sqlite> select * from primes;
```

## Delete

`DELETE` 语法（syntax diagram）：

```
DELETE FROM qualified-table-name [WHERE expr]
```

Delete removes some or all rows from a table.

终端例子（删除前）：

```
sqlite> select * from primes;
2|1
3|1
4|0
5|1
6|0
7|1
8|0
9|0
10|0
11|1
12|0
13|1
14|0
15|0
16|0
17|1
18|0
19|1
20|0
21|0
22|0
23|1
24|0
25|0
sqlite> DELETE FROM primes WHERE prime=0;
sqlite>
```

终端例子（删除后）：

```
sqlite> select * from primes;
2|1
3|1
5|1
7|1
11|1
13|1
17|1
19|1
23|1
sqlite>
```

---

# Python and SQL

```python
import sqlite3

db = sqlite3.Connection("n.db")
db.execute("CREATE TABLE nums AS SELECT 2 UNION SELECT 3;")
db.execute("INSERT INTO nums VALUES (?), (?), (?);", range(4, 7))
print(db.execute("SELECT * FROM nums;").fetchall())
db.commit()
```

对应终端：

```
~/lec$ python3 ex.py
[(2,), (3,), (4,), (5,), (6,)]
~/lec$ ls n.db
n.db
~/lec$ sqlite3 n.db
SQLite version 3.19.3 2017-06-27 16:48:08
Enter ".help" for usage hints.
sqlite> SELECT * FROM nums;
2
3
4
5
6
sqlite>
```

sqlite3 可以直接 import

sqlite3 有个 class：connection

然后去 execute 可以使用 python 语法，如 range，然后填到前面占位符那里

fetchall（）可以让其全部显示在一个 list 里

commit 就是提交到这个文件中

## Related

- [[SQL]]
- [[Tables]]
- [[Aggregation]]

---

# 截图

![[截屏2026-08-09 13.16.09.png]]

![[截屏2026-08-09 13.16.59.png]]

![[截屏2026-08-12 13.40.22.png]]

![[截屏2026-08-12 13.42.32.png]]

![[截屏2026-08-12 13.43.24.png]]

![[截屏2026-08-12 13.45.09.png]]

![[截屏2026-08-12 13.45.38.png]]

![[截屏2026-08-12 13.46.20.png]]

![[截屏2026-08-12 13.46.35.png]]

![[截屏2026-08-13 10.58.41.png]]
