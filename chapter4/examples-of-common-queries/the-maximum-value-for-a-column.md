### 4.6.1 The Maximum Value for a Column

“最高（最低）的物品号是多少？”

```
SELECT MAX(article) AS article FROM shop;
+---------+
| article |
+---------+
|       4 |
+---------+
```

---

==来自评论区==

最低则用 MIN\(\)

平均则用 AVG\(\)

求和则用 SUM\(\)

