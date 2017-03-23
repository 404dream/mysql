### 4.6.4 The Rows Holding the Group-wise Maximum of a Certain Column

任务：找到最贵价格的文章的经销商。

```
SELECT article, dealer, price
FROM   shop s1
WHERE  price=(SELECT MAX(s2.price)
              FROM shop s2
              WHERE s1.article = s2.article);

+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0001 | B      |  3.99 |
|    0002 | A      | 10.99 |
|    0003 | C      |  1.69 |
|    0004 | D      | 19.95 |
+---------+--------+-------+
```

上述示例使用相关的子查询，可能无效（请参见第14.2.10.7节“相关子查询”）。解决问题的其他可能性是在FROM或LEFT JOIN语句中使用不相关的子查询。

不相关的子查询：

```
SELECT s1.article, dealer, s1.price
FROM shop s1
JOIN (
  SELECT article, MAX(price) AS price
  FROM shop
  GROUP BY article) AS s2
  ON s1.article = s2.article AND s1.price = s2.price;
```

LEFT JOIN：

```
SELECT s1.article, s1.dealer, s1.price
FROM shop s1
LEFT JOIN shop s2 ON s1.article = s2.article AND s1.price 
<
 s2.price
WHERE s2.article IS NULL;
```

LEFT JOIN的工作原理是，当s1.price处于其最大值时，没有s2.price具有更大的值，s2行的值将为NULL。参见第14.2.9.2节“加入语法”。

