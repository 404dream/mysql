#### 4.3.4.7 Pattern Matching

MySQL提供标准的SQL模式匹配以及一种基于扩展正则表达式的模式匹配形式，类似于vi，grep和sed等Unix实用程序所使用的模式匹配。

SQL模式匹配允许使用 \_ 匹配任何单个字符，% 可匹配任意数目的字符（包括零个字符）。在MySQL中，SQL模式默认情况下不区分大小写。这里显示了一些示例。在使用SQL模式时，不要使用=或&lt;&gt;;请使用LIKE或NOT LIKE比较运算符。

要查找以b开头的名称：

```
mysql>SELECT * FROM pet WHERE name LIKE 'b%';
+--------+--------+---------+------+------------+------------+
| name   | owner  | species | sex  | birth      | death      |
+--------+--------+---------+------+------------+------------+
| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL       |
| Bowser | Diane  | dog     | m    | 1989-08-31 | 1995-07-29 |
+--------+--------+---------+------+------------+------------+
```

要查找以fy结尾的名称：

```
mysql>SELECT * FROM pet WHERE name LIKE '%fy';
+--------+--------+---------+------+------------+-------+
| name   | owner  | species | sex  | birth      | death |
+--------+--------+---------+------+------------+-------+
| Fluffy | Harold | cat     | f    | 1993-02-04 | NULL  |
| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL  |
+--------+--------+---------+------+------------+-------+
```

要查找以包含w字符的名称：

```
mysql>SELECT * FROM pet WHERE name LIKE '%w%';
+----------+-------+---------+------+------------+------------+
| name     | owner | species | sex  | birth      | death      |
+----------+-------+---------+------+------------+------------+
| Claws    | Gwen  | cat     | m    | 1994-03-17 | NULL       |
| Bowser   | Diane | dog     | m    | 1989-08-31 | 1995-07-29 |
| Whistler | Gwen  | bird    | NULL | 1997-12-09 | NULL       |
+----------+-------+---------+------+------------+------------+
```

要查找正好包含五个字符的名称，请使用五个\_ pattern字符的实例：

```
mysql>SELECT * FROM pet WHERE name LIKE '_____';
+-------+--------+---------+------+------------+-------+
| name  | owner  | species | sex  | birth      | death |
+-------+--------+---------+------+------------+-------+
| Claws | Gwen   | cat     | m    | 1994-03-17 | NULL  |
| Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
+-------+--------+---------+------+------------+-------+
```

MySQL提供的其他类型的模式匹配使用扩展的正则表达式。当您测试此类型模式的匹配时，请使用REGEXP和NOT REGEXP运算符（或RLIKE和NOT RLIKE，它们是同义词）。

以下列表描述了扩展正则表达式的一些特征：

* . 匹配任何单个字符。
* 字符类\[...\]匹配括号内的任何字符。例如，\[abc\]匹配a，b或c。要规定字符范围，请使用破折号。 \[a-z\]匹配任何字母，而\[0-9\]匹配任何数字。

* \*匹配零个或多个对象实例。例如，x \*匹配任意数量的x个字符，\[0-9\] \*匹配任何数量的数字，而\*匹配任何数量的任何数量。

* 如果模式匹配在测试值中的任何位置匹配则REGEXP模式匹配成功。 （这与LIKE模式匹配不同，后者仅在模式匹配整个值时才成功）。

* 要锚定模式以使其必须匹配要测试的值的开头或结尾，请在模式开头使用^或在模式结尾使用$

为了演示扩展正则表达式如何工作，先前显示的LIKE查询使用REGEXP重写。

要查找以b开头的名称，请使用^匹配名称的开头：

```
mysql> SELECT * FROM pet WHERE name REGEXP '^b';
+--------+--------+---------+------+------------+------------+
| name   | owner  | species | sex  | birth      | death      |
+--------+--------+---------+------+------------+------------+
| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL       |
| Bowser | Diane  | dog     | m    | 1989-08-31 | 1995-07-29 |
+--------+--------+---------+------+------------+------------+
```

如果你真的想强制REGEXP比较区分大小写，使用BINARY关键字使其中一个字符串成为二进制字符串。此查询仅匹配名称开头的小写b：

```
mysql>SELECT * FROM pet WHERE name REGEXP BINARY '^b';
```

要查找以fy结尾的名称，请使用$匹配名称的结尾：

```
mysql>SELECT * FROM pet WHERE name REGEXP 'fy$';
+--------+--------+---------+------+------------+-------+
| name   | owner  | species | sex  | birth      | death |
+--------+--------+---------+------+------------+-------+
| Fluffy | Harold | cat     | f    | 1993-02-04 | NULL  |
| Buffy  | Harold | dog     | f    | 1989-05-13 | NULL  |
+--------+--------+---------+------+------------+-------+
```

要查找包含w的名称，请使用以下查询：

```
mysql>SELECT * FROM pet WHERE name REGEXP 'w';
+----------+-------+---------+------+------------+------------+
| name     | owner | species | sex  | birth      | death      |
+----------+-------+---------+------+------------+------------+
| Claws    | Gwen  | cat     | m    | 1994-03-17 | NULL       |
| Bowser   | Diane | dog     | m    | 1989-08-31 | 1995-07-29 |
| Whistler | Gwen  | bird    | NULL | 1997-12-09 | NULL       |
+----------+-------+---------+------+------------+------------+
```

要查找包含完全五个字符的名称，请使用^和$匹配名称的开头和结尾，以及五个实例。介于：

```
mysql>SELECT * FROM pet WHERE name REGEXP '^.....$';
+-------+--------+---------+------+------------+-------+
| name  | owner  | species | sex  | birth      | death |
+-------+--------+---------+------+------------+-------+
| Claws | Gwen   | cat     | m    | 1994-03-17 | NULL  |
| Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
+-------+--------+---------+------+------------+-------+
```

您还可以使用{n}（“repeat-n-times”）运算符重写上一个查询：

```
mysql>SELECT * FROM pet WHERE name REGEXP '^.{5}$';
+-------+--------+---------+------+------------+-------+
| name  | owner  | species | sex  | birth      | death |
+-------+--------+---------+------+------------+-------+
| Claws | Gwen   | cat     | m    | 1994-03-17 | NULL  |
| Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
+-------+--------+---------+------+------------+-------+
```

第13.5.2节“正则表达式”提供了有关正则表达式语法的更多信息。
