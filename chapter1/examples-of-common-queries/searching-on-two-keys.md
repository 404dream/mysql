### 4.6.7 Searching on Two Keys

使用单个key的OR和AND被优化，一个棘手的情况是在两个不同的key上使用OR语句：

```
SELECT field1_index, field2_index FROM test_table
WHERE field1_index = '1' OR  field2_index = '1'
```

MySql对这种情况进行了优化。请参见第9.2.1.3节“索引合并优化”。

您还可以通过使用组合两个单独的SELECT语句的输出的UNION来有效地解决问题。请参见第14.2.9.3节“UNION语法”。每个SELECT只搜索一个键并可以进行优化：

```
SELECT field1_index, field2_index
    FROM test_table WHERE field1_index = '1'
UNION
SELECT field1_index, field2_index
    FROM test_table WHERE field2_index = '1';
```



