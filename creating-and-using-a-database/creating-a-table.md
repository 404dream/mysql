### 4.3.2 Creating a Table

创建数据库是很简单的，但是通过SHOW TABLES 命令行告诉我们这个数据库是空的：

```
mysql>SHOW TABLES;
Empty set (0.00 sec)
```

更困难的部分是决定数据库的结构应该是什么：你需要什么表格，每个列应该是什么列。

您需要一个包含每个pets的记录的表。这可以被称为pet表，作为最低限度，它应该包含每个动物的名字。名称本身并不十分有趣，表应该包含其他信息。例如，如果你家里不止一个人养宠物，你可能想列出每个动物的主人。你还可以记录一些基本的描述性信息，如物种和性别。

年龄如何？这可能是有兴趣的，但它不是一件好事，存储在数据库中。随着时间的推移，年龄的变化，这意味着你必须经常更新你的记录。相反，它更好地存储一个固定的值，如出生日期。然后，每当你需要年龄，你可以计算它作为当前日期和出生日期之间的差异。MySQL提供的功能，做日期算术，所以这并不困难。储存出生日期而不是年龄还有其他好处：

* 您可以使用数据库的任务，如生成提醒即将到来的宠物生日。（如果您认为这种类型的查询有点傻，请注意，这是同一个问题，您可能会要求在一个商业数据库的背景下，以确定客户，您需要发送生日问候在当前一周或一个月内，为电脑辅助个人接触。）

* 您可以计算年龄相对于日期以外的日期。例如，如果你在数据库中存储死亡日期，你可以很容易的计算出宠物死后有多大。

您可能会想到对于pet表其他有用的信息，但迄今确定的是足够的：名称，所有者，物种，性别，出生和死亡

```
mysql>CREATE TABLE pet (name VARCHAR(20), owner VARCHAR(20),
    ->species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
```

对于名称、物种、和所有者三列因为列值的长度不同VARCHAR是个不错的选择。这些列定义中的长度不一定都是相同的，不必是20。你通常可以选择任何长度从1到65535。如果你做了一个糟糕的选择，后来发现你需要一个较长的字段，MySQL提供了一个ALTER TABLE语句。

几种类型的值可以被选择来代表性动物记录，如“m”和“f”，或“male”和“female”。使用单个字符“m”和“f”是最简单的。

使用DATE数据类型的出生和死亡列是一个相当明显的选择。

一旦创建表后，执行SHOW TABLES命令会输出如下信息：

```
mysql>SHOW TABLES;
+---------------------+
| Tables in menagerie |
+---------------------+
| pet                 |
+---------------------+
```

要验证您的表是按照您预期的方式创建的，请使用DESCRIBE语句：

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

您可以在任何时候使用DESCRIBE语句，比如您忘记了列名称或表的其他信息。

关于更多的其他信息，您可以查看MySql data types章节：[Chapter 12,Data Types](https://dev.mysql.com/doc/refman/5.7/en/data-types.html).
