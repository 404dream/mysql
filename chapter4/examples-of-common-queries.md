## 4.6 Examples of Common Queries

[4.6.1 The Maximum Value for a Column](https://dev.mysql.com/doc/refman/5.7/en/example-maximum-column.html)

[4.6.2 The Row Holding the Maximum of a Certain Column](https://dev.mysql.com/doc/refman/5.7/en/example-maximum-row.html)

[4.6.3 Maximum of Column per Group](https://dev.mysql.com/doc/refman/5.7/en/example-maximum-column-group.html)

[4.6.4 The Rows Holding the Group-wise Maximum of a Certain Column](https://dev.mysql.com/doc/refman/5.7/en/example-maximum-column-group-row.html)

[4.6.5 Using User-Defined Variables](https://dev.mysql.com/doc/refman/5.7/en/example-user-variables.html)

[4.6.6 Using Foreign Keys](https://dev.mysql.com/doc/refman/5.7/en/example-foreign-keys.html)

[4.6.7 Searching on Two Keys](https://dev.mysql.com/doc/refman/5.7/en/searching-on-two-keys.html)

[4.6.8 Calculating Visits Per Day](https://dev.mysql.com/doc/refman/5.7/en/calculating-days.html)

[4.6.9 Using AUTO\_INCREMENT](https://dev.mysql.com/doc/refman/5.7/en/example-auto-increment.html)

以下是如何解决MySQL的一些常见问题的示例。

Some of the examples use the table`shop`to hold the price of each article \(item number\) for certain traders \(dealers\). Supposing that each trader has a single fixed price per article, then \(`article`,`dealer`\) is a primary key for the records.

启动命令行工具mysql并选择一个数据库：

```
shell>mysql your-database-name
```

（在大多数MySQL安装中，可以使用名为test的数据库）。

您可以使用以下语句创建并填充示例表：

```
 CREATE TABLE shop (
    article INT(4) UNSIGNED ZEROFILL DEFAULT '0000' NOT NULL,
    dealer  CHAR(20)                 DEFAULT ''     NOT NULL,
    price   DOUBLE(16,2)             DEFAULT '0.00' NOT NULL,
    PRIMARY KEY(article, dealer));
INSERT INTO shop VALUES
    (1,'A',3.45),(1,'B',3.99),(2,'A',10.99),(3,'B',1.45),
    (3,'C',1.69),(3,'D',1.25),(4,'D',19.95);
```

执行后，该表应具有以下内容：

```
SELECT * FROM shop;
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0001 | A      |  3.45 |
|    0001 | B      |  3.99 |
|    0002 | A      | 10.99 |
|    0003 | B      |  1.45 |
|    0003 | C      |  1.69 |
|    0003 | D      |  1.25 |
|    0004 | D      | 19.95 |
+---------+--------+-------+
```



