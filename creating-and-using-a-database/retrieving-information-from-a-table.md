### 4.3.4 Retrieving Information from a Table

[4.3.4.1 Selecting All Data](https://dev.mysql.com/doc/refman/5.7/en/selecting-all.html)

[4.3.4.2 Selecting Particular Rows](https://dev.mysql.com/doc/refman/5.7/en/selecting-rows.html)

[4.3.4.3 Selecting Particular Columns](https://dev.mysql.com/doc/refman/5.7/en/selecting-columns.html)

[4.3.4.4 Sorting Rows](https://dev.mysql.com/doc/refman/5.7/en/sorting-rows.html)

[4.3.4.5 Date Calculations](https://dev.mysql.com/doc/refman/5.7/en/date-calculations.html)

[4.3.4.6 Working with NULL Values](https://dev.mysql.com/doc/refman/5.7/en/working-with-null.html)

[4.3.4.7 Pattern Matching](https://dev.mysql.com/doc/refman/5.7/en/pattern-matching.html)

[4.3.4.8 Counting Rows](https://dev.mysql.com/doc/refman/5.7/en/counting-rows.html)

[4.3.4.9 Using More Than one Table](https://dev.mysql.com/doc/refman/5.7/en/multiple-tables.html)

SELECT语句用于从表中提取信息。语句的一般形式是：

```
SELECT what_to_select
FROM which_table
WHERE conditions_to_satisfy;
```

what\_to\_select指示您想要查看的内容。这可以一个或多个列的名称用“,”隔开，或\*表示“所有列”。which\_table指示要从中检索数据的表。 WHERE子句是可选的。如果存在，则conditions\_to\_satisfy指定行必须满足以满足检索要求的一个或多个条件。

