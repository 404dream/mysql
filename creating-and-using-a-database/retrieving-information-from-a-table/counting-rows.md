#### 4.3.4.8 Counting Rows

数据库通常用于回答这样一个问题：“表中某种类型的数据出现的频率是多少？”例如，您可能想知道您拥有多少只宠物，或者每位拥有者拥有多少只宠物，或者您可能想要为您的动物进行各种普查操作。

统计你拥有的动物总数和“宠物表中有多少行？”是相同的问题，因为每只宠物有一个记录。 COUNT\(\*\)计算行数，因此计数动物的查询如下所示：

```
mysql>SELECT COUNT(*) FROM pet;
+----------+
| COUNT(*) |
+----------+
|        9 |
+----------+
```

之前，你检索了拥有宠物的人的名字。如果您想知道每位拥有者拥有多少宠物，可以使用COUNT\(\)：

```
mysql>SELECT owner, COUNT(*) FROM pet GROUP BY owner;
+--------+----------+
| owner  | COUNT(*) |
+--------+----------+
| Benny  |        2 |
| Diane  |        2 |
| Gwen   |        3 |
| Harold |        2 |
+--------+----------+
```

前面的查询使用GROUP BY来为每个所有者分组所有记录。将COUNT\(\)与GROUP BY结合使用对于在不同分组下表征您的数据非常有用。以下示例显示了宠物普查操作的不同方法。

每类宠物的个数：

```
mysql>SELECT species, COUNT(*) FROM pet GROUP BY species;
+---------+----------+
| species | COUNT(*) |
+---------+----------+
| bird    |        2 |
| cat     |        2 |
| dog     |        3 |
| hamster |        1 |
| snake   |        1 |
+---------+----------+
```

每种性别的宠物数：

```
mysql>SELECT sex, COUNT(*) FROM pet GROUP BY sex;
+------+----------+
| sex  | COUNT(*) |
+------+----------+
| NULL |        1 |
| f    |        4 |
| m    |        4 |
+------+----------+
```

（在此输出中，NULL表示性别未知。）

每类宠物和性别的组合的数量：

```
mysql>SELECT species, sex, COUNT(*) FROM pet GROUP BY species, sex;
+---------+------+----------+
| species | sex  | COUNT(*) |
+---------+------+----------+
| bird    | NULL |        1 |
| bird    | f    |        1 |
| cat     | f    |        1 |
| cat     | m    |        1 |
| dog     | f    |        1 |
| dog     | m    |        2 |
| hamster | f    |        1 |
| snake   | m    |        1 |
+---------+------+----------+
```

当您使用COUNT\(\)时，不需要检索整个表。例如，上面的查询，当只对狗和猫执行时，看起来像这样：

```
mysql>SELECT species, sex, COUNT(*) FROM pet
    ->WHERE species = 'dog' OR species = 'cat'
    ->GROUP BY species, sex;
+---------+------+----------+
| species | sex  | COUNT(*) |
+---------+------+----------+
| cat     | f    |        1 |
| cat     | m    |        1 |
| dog     | f    |        1 |
| dog     | m    |        2 |
+---------+------+----------+
```

或者，如果你想要每个性别的动物数量只有已知性别的动物：

```
mysql>SELECT species, sex, COUNT(*) FROM pet
    ->WHERE sex IS NOT NULL
    ->GROUP BY species, sex;
+---------+------+----------+
| species | sex  | COUNT(*) |
+---------+------+----------+
| bird    | f    |        1 |
| cat     | f    |        1 |
| cat     | m    |        1 |
| dog     | f    |        1 |
| dog     | m    |        2 |
| hamster | f    |        1 |
| snake   | m    |        1 |
+---------+------+----------+
```

如果除了COUNT\(\)值之外还有其他要选择的列，则需要添加一个GROUP BY子句。否则，会发生以下情况：

* 如果启用ONLY\_FULL\_GROUP\_BY SQL模式，则会发生错误：

```
mysql>SET sql_mode = 'ONLY_FULL_GROUP_BY';
Query OK, 0 rows affected (0.00 sec)
mysql>SELECT owner, COUNT(*) FROM pet;
ERROR 1140 (42000): In aggregated query without GROUP BY, expression
#1 of SELECT list contains nonaggregated column 'menagerie.pet.owner';
this is incompatible with sql_mode=only_full_group_by
```

* 如果未启用ONLY\_FULL\_GROUP\_BY，则通过将所有行视为单个组来处理查询，但为每个列的名称选择的值不确定。服务器可以从任意行中自由选择值：

```
mysql>SET sql_mode = '';
Query OK, 0 rows affected (0.00 sec)
mysql>SELECT owner, COUNT(*) FROM pet;
+--------+----------+
| owner  | COUNT(*) |
+--------+----------+
| Harold |        8 |
+--------+----------+
1 row in set (0.00 sec)
```

另请参见第13.19.3节“GROUP BY的MySQL处理”。有关COUNT（expr）行为和相关优化的信息，请参见第13.19.1节“聚合（GROUP BY）功能说明”。

