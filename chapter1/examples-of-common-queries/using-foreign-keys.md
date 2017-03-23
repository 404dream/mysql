### 4.6.6 Using Foreign Keys

在MySQL中，InnoDB表支持检查外键约束。请参见第15章“InnoDB存储引擎”和第1.8.2.3节“外键差异”。

外键约束不仅仅需要连接两个表。对于除InnoDB之外的存储引擎，可以在定义列时使用REFERENCES tbl\_name（col\_name）语句，该参数没有实际的影响，仅作为备注或注释，您当前定义的列旨在引用另一个表中的列。使用这种语法时，理解这一点非常重要：

* MySQL不执行任何类型的CHECK来确保col\_name实际上存在于tbl\_name（或者甚至t​​bl\_name本身存在）中。

* MySQL不对tbl\_name执行任何类型的操作，例如删除行以响应您正在定义的表中的行执行的操作;换句话说，这种语法不会导致任何ON DELETE或ON UPDATE行为。 （虽然可以将ON DELETE或ON UPDATE语句写入REFERENCES语句中，但也被忽略。）

* 此语法只创建列;它不会创建任何类型的索引或键。

您可以使用作为连接列的列，如下所示：

```
CREATE TABLE person (
    id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
    name CHAR(60) NOT NULL,
    PRIMARY KEY (id)
);

CREATE TABLE shirt (
    id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
    style ENUM('t-shirt', 'polo', 'dress') NOT NULL,
    color ENUM('red', 'blue', 'orange', 'white', 'black') NOT NULL,
    owner SMALLINT UNSIGNED NOT NULL REFERENCES person(id),
    PRIMARY KEY (id)
);

INSERT INTO person VALUES (NULL, 'Antonio Paz');

SELECT @last := LAST_INSERT_ID();

INSERT INTO shirt VALUES
(NULL, 'polo', 'blue', @last),
(NULL, 'dress', 'white', @last),
(NULL, 't-shirt', 'blue', @last);

INSERT INTO person VALUES (NULL, 'Lilliana Angelovska');

SELECT @last := LAST_INSERT_ID();

INSERT INTO shirt VALUES
(NULL, 'dress', 'orange', @last),
(NULL, 'polo', 'red', @last),
(NULL, 'dress', 'blue', @last),
(NULL, 't-shirt', 'white', @last);

SELECT * FROM person;
+----+---------------------+
| id | name                |
+----+---------------------+
|  1 | Antonio Paz         |
|  2 | Lilliana Angelovska |
+----+---------------------+

SELECT * FROM shirt;
+----+---------+--------+-------+
| id | style   | color  | owner |
+----+---------+--------+-------+
|  1 | polo    | blue   |     1 |
|  2 | dress   | white  |     1 |
|  3 | t-shirt | blue   |     1 |
|  4 | dress   | orange |     2 |
|  5 | polo    | red    |     2 |
|  6 | dress   | blue   |     2 |
|  7 | t-shirt | white  |     2 |
+----+---------+--------+-------+


SELECT s.* FROM person p INNER JOIN shirt s
   ON s.owner = p.id
 WHERE p.name LIKE 'Lilliana%'
   AND s.color <>'white';

+----+-------+--------+-------+
| id | style | color  | owner |
+----+-------+--------+-------+
|  4 | dress | orange |     2 |
|  5 | polo  | red    |     2 |
|  6 | dress | blue   |     2 |
+----+-------+--------+-------+
```

当以这种方式使用时，REFERENCES语句不会显示在SHOW CREATE TABLE或DESCRIBE的输出中：

    SHOW CREATE TABLE shirt\G
    *************************** 1. row ***************************
    Table: shirt
    Create Table: CREATE TABLE `shirt` (
    `id` smallint(5) unsigned NOT NULL auto_increment,
    `style` enum('t-shirt','polo','dress') NOT NULL,
    `color` enum('red','blue','orange','white','black') NOT NULL,
    `owner` smallint(5) unsigned NOT NULL,
    PRIMARY KEY  (`id`)
    ) ENGINE=MyISAM DEFAULT CHARSET=latin1

使用REFERENCES以此方式作为列定义中的注释或“提醒”可与MyISAM表配合使用。

