## Getting Information About Databases and Tables

如果忘记了数据库或表的名称，或者给定表的结构是什么（例如，它的列是什么），该怎么办？ MySQL通过提供有关其支持的数据库和表的信息的几个语句来解决这个问题。

您以前看过SHOW DATABASES，其中列出了由服务器管理的数据库。要查找当前选择的数据库，请使用DATABASE\(\)函数：

```
mysql>SELECT DATABASE();
+------------+
| DATABASE() |
+------------+
| menagerie  |
+------------+
```

如果您尚未选择任何数据库，则结果为NULL。

要找出默认数据库包含的表（例如，当您不确定表的名称）时，请使用以下语句：

```
mysql>SHOW TABLES;
+---------------------+
| Tables_in_menagerie |
+---------------------+
| event               |
| pet                 |
+---------------------+
```

此语句生成的输出中的列名称始终为Tables\_in\_db\_name，其中db\_name是数据库的名称。有关详细信息，请参见第14.7.5.37节“显示表语法”。

如果你想了解一个表的结构，DESCRIBE语句是有用的;它显示有关每个表的列的信息：

```
mysql>DESCRIBE pet;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | YES  |     | NULL    |       |
| owner   | varchar(20) | YES  |     | NULL    |       |
| species | varchar(20) | YES  |     | NULL    |       |
| sex     | char(1)     | YES  |     | NULL    |       |
| birth   | date        | YES  |     | NULL    |       |
| death   | date        | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
```

Fieldbiao列名称，Type是列的数据类型，NULL表示列是否可以包含NULL值，Key表示列是否已建立索引，“Default”指定列的默认值。 “Extra ”显示有关列的特殊信息：如果使用AUTO\_INCREMENT选项创建列，则该值将为auto\_increment而不是空。

DESC是DESCRIBE的简称。有关详细信息，请参见第14.8.1节“描述语法”。

您可以使用SHOW CREATE TABLE语句获取创建现有表所需的CREATE TABLE语句。请参见第14.7.5.10节“SHOW CREATE TABLE语法”。

如果您在表上有索引，SHOW INDEX FROM tbl\_name会生成有关它们的信息。有关此语句的更多信息，请参见第14.7.5.22节“显示索引语法”。

