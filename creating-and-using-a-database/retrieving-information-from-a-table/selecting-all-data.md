#### 4.3.4.1 Selecting All Data

SELECT的最简单形式是从表中检索所有内容：

```
mysql>SELECT * FROM pet;
+----------+--------+---------+------+------------+------------+
| name     | owner  | species | sex  | birth      | death      |
+----------+--------+---------+------+------------+------------+
| Fluffy   | Harold | cat     | f    | 1993-02-04 | NULL       |
| Claws    | Gwen   | cat     | m    | 1994-03-17 | NULL       |
| Buffy    | Harold | dog     | f    | 1989-05-13 | NULL       |
| Fang     | Benny  | dog     | m    | 1990-08-27 | NULL       |
| Bowser   | Diane  | dog     | m    | 1979-08-31 | 1995-07-29 |
| Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
| Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
| Slim     | Benny  | snake   | m    | 1996-04-29 | NULL       |
| Puffball | Diane  | hamster | f    | 1999-03-30 | NULL       |
+----------+--------+---------+------+------------+------------+
```

在刚刚加载初始数据集之后，需要查看整张表内容，此形式的SELECT很有用。例如，你可能会认为Bowser的出生日期似乎不太对。查看原始数据，你发现正确的出生年份应该是1989年，而不是1979年。

有至少两种方法来解决这个问题：

* 编辑pet.txt文件以更正错误，然后清空表并使用DELETE和LOAD DATA重新加载：

但是，如果您这样做，您还必须重新输入Puffball的记录。

* 使用UPDATE语句仅仅修复错误的记录：

```
mysql>UPDATE pet SET birth = '1989-08-31' WHERE name = 'Bowser';
```

UPDATE只更改相关记录，不需要重新加载表。

