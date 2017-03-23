#### 4.3.4.9 Using More Than one Table

宠物表跟踪你有哪些宠物。如果你想记录关于他们的其他信息，例如在他们的生活中的事件，如访问兽医或出生时，你需要另一个表。这个表应该是什么样子？它需要包含以下信息：

* 宠物名字，让你知道每个事件属于哪个动物。
* 日期，以便您知道事件发生的时间。
* 用于描述事件的字段。
* 如果您想要能够对事件进行分类需要设置事件类型字段。

鉴于这些注意事项，事件表的CREATE TABLE语句可能如下所示：

```
mysql>CREATE TABLE event (name VARCHAR(20), date DATE,
    ->type VARCHAR(15), remark VARCHAR(255));
```

与宠物表一样，通过创建包含以下信息的制表符分隔的文本文件来加载初始记录是最容易的。

| name | date | type | remark |
| :--- | :--- | :--- | :--- |
| Fluffy | 1995-05-15 | litter | 4 kittens, 3 female, 1 male |
| Buffy | 1993-06-23 | litter | 5 puppies, 2 female, 3 male |
| Buffy | 1994-06-19 | litter | 3 puppies, 3 female |
| Chirpy | 1999-03-21 | vet | needed beak straightened |
| Slim | 1997-08-03 | vet | broken rib |
| Bowser | 1991-10-12 | kennel |  |
| Fang | 1991-10-12 | kennel |  |
| Fang | 1998-08-28 | birthday | Gave him a new chew toy |
| Claws | 1998-03-17 | birthday | Gave him a new flea collar |
| Whistler | 1998-12-09 | birthday | First birthday |

按照如下命令加载记录：

```
mysql>LOAD DATA LOCAL INFILE 'event.txt' INTO TABLE event;
```

基于您从在宠物表上运行的查询中学到的内容，您应该能够对事件表中的记录执行检索;原则是一样的。但是什么时候事件表本身不足以回答你可能会问的问题？

假设你想知道有崽崽的宠物年龄。我们之前看到如何计算两个日期的年龄。母亲的产仔日期在事件表中，但要计算她的年龄，您需要她的出生日期，该日期存储在宠物表中。这意味着查询需要两个表：

```
mysql>SELECT pet.name,
    ->TIMESTAMPDIFF(YEAR,birth,date) AS age,
    ->remark
    ->FROM pet INNER JOIN event
    ->  ON pet.name = event.name
    ->WHERE event.type = 'litter';
+--------+------+-----------------------------+
| name   | age  | remark                      |
+--------+------+-----------------------------+
| Fluffy |    2 | 4 kittens, 3 female, 1 male |
| Buffy  |    4 | 5 puppies, 2 female, 3 male |
| Buffy  |    5 | 3 puppies, 3 female         |
+--------+------+-----------------------------+
```

有关此查询的几个注意事项：

* FROM子句连接两个表，因为查询需要从两个表中提取信息。
* 在合并（连接）多个表中的信息时，您需要指定一个表中的记录如何与另一个表中的记录匹配。这很容易，因为它们都有一个名称列。查询使用ON子句根据名称值匹配两个表中的记录。查询使用INNER JOIN组合表。当且仅当两个表都满足ON子句中指定的条件时，INNER JOIN允许每个表中的行出现在结果中。在此示例中，ON子句指定pet表中的名称列必须与事件表中的名称列匹配。如果一个名称出现在一个表中但不在另一个表中，则该行不会出现在结果中，因为ON子句中的条件失败。
* 因为名称列出现在两个表中，所以在引用列时，必须明确指出哪个表。这可以通过在表名前添加列名来实现。

您不需要有两个不同的表来执行连接。有时，如果要将表中的记录与同一个表中的其他记录进行比较，则将表连接到自己是有用的。例如，要在您的宠物中找到繁殖对，您可以加入宠物表与自己，以产生候选对的类似物种的男性和女性：

```
mysql>SELECT p1.name, p1.sex, p2.name, p2.sex, p1.species
    ->FROM pet AS p1 INNER JOIN pet AS p2
    ->ON p1.species = p2.species AND p1.sex = 'f' AND p2.sex = 'm';
+--------+------+--------+------+---------+
| name   | sex  | name   | sex  | species |
+--------+------+--------+------+---------+
| Fluffy | f    | Claws  | m    | cat     |
| Buffy  | f    | Fang   | m    | dog     |
| Buffy  | f    | Bowser | m    | dog     |
+--------+------+--------+------+---------+
```

在此查询中，我们为表名指定别名以引用列，并保持每个列引用与之相关联的表的哪个实例。

