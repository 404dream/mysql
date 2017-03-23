#### 4.3.4.3 Selecting Particular Columns

如果您不想查看表格中的所有列，只需指定感兴趣的列，以逗号分隔。例如，如果您想知道您的动物出生的时间，请选择姓名和出生栏：

```
mysql>SELECT name, birth FROM pet;
+----------+------------+
| name     | birth      |
+----------+------------+
| Fluffy   | 1993-02-04 |
| Claws    | 1994-03-17 |
| Buffy    | 1989-05-13 |
| Fang     | 1990-08-27 |
| Bowser   | 1989-08-31 |
| Chirpy   | 1998-09-11 |
| Whistler | 1997-12-09 |
| Slim     | 1996-04-29 |
| Puffball | 1999-03-30 |
+----------+------------+
```

要了解谁拥有宠物，请使用以下查询：

```
mysql>SELECT owner FROM pet;
+--------+
| owner  |
+--------+
| Harold |
| Gwen   |
| Harold |
| Benny  |
| Diane  |
| Gwen   |
| Gwen   |
| Benny  |
| Diane  |
+--------+
```

请注意，查询只是从每个记录中检索所有者列，其中一些显示不止一次。要最小化输出，请通过添加关键字DISTINCT检索每个唯一输出记录一次：

```
mysql>SELECT DISTINCT owner FROM pet;
+--------+
| owner  |
+--------+
| Benny  |
| Diane  |
| Gwen   |
| Harold |
+--------+
```

您可以使用WHERE语句将行选择与列选择组合。例如，要仅为狗和猫生成出生日期，请使用以下查询：

```
mysql>SELECT name, species, birth FROM pet
    ->WHERE species = 'dog' OR species = 'cat';
+--------+---------+------------+
| name   | species | birth      |
+--------+---------+------------+
| Fluffy | cat     | 1993-02-04 |
| Claws  | cat     | 1994-03-17 |
| Buffy  | dog     | 1989-05-13 |
| Fang   | dog     | 1990-08-27 |
| Bowser | dog     | 1989-08-31 |
+--------+---------+------------+
```



