### 4.6.8 Calculating Visits Per Day

以下示例显示了如何使用位组功能来计算用户访问网页每月的天数。

```
CREATE TABLE t1 (year YEAR(4), month INT(2) UNSIGNED ZEROFILL,
            day INT(2) UNSIGNED ZEROFILL);
INSERT INTO t1 VALUES(2000,1,1),(2000,1,20),(2000,1,30),(2000,2,2),
            (2000,2,23),(2000,2,23);
```

示例表包含表示用户访问页面的年月日值。要确定这些访问发生在每个月中有多少个不同的日期，请使用此查询：

```
SELECT year,month,BIT_COUNT(BIT_OR(1<<day)) AS days FROM t1 GROUP BY year,month;
```

返回结果：

```
+------+-------+------+
| year | month | days |
+------+-------+------+
| 2000 |    01 |    3 |
| 2000 |    02 |    2 |
+------+-------+------+
```

该查询计算每个年/月组合中表格中显示的不同天数，自动删除重复的条目。

