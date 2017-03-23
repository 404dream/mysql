### 4.6.3 Maximum of Column per Group

任务：找到每篇文章最高的价格。

```
SELECT article, MAX(price) AS price
FROM   shop
GROUP BY article;
+---------+-------+
| article | price |
+---------+-------+
|    0001 |  3.99 |
|    0002 | 10.99 |
|    0003 |  1.69 |
|    0004 | 19.95 |
+---------+-------+
```



