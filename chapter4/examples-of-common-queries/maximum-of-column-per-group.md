### 4.6.2 The Row Holding the Maximum of a Certain Column

任务：查找最贵文章的数量、经销商和价格。

```
SELECT article, dealer, priceFROM   shop
WHERE  price=(SELECT MAX(price) FROM shop);
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0004 | D      | 19.95 |
+---------+--------+-------+
```

其他解决方案是使用LEFT JOIN或按价格降序排序所有行，并使用MySQL特定的LIMIT语句仅获取第一行：

```
SELECT s1.article, s1.dealer, s1.price FROM shop s1
LEFT JOIN shop s2 ON s1.price < s2.price
WHERE s2.article IS NULL;

SELECT article, dealer, price
FROM shop
ORDER BY price DESC
LIMIT 1;
```



