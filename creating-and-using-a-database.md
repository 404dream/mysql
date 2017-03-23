## 4.3 Creating and Using a Database

[4.3.1 Creating and Selecting a Database](https://dev.mysql.com/doc/refman/5.7/en/creating-database.html)

[4.3.2 Creating a Table](https://dev.mysql.com/doc/refman/5.7/en/creating-tables.html)

[4.3.3 Loading Data into a Table](https://dev.mysql.com/doc/refman/5.7/en/loading-tables.html)

[4.3.4 Retrieving Information from a Table](https://dev.mysql.com/doc/refman/5.7/en/retrieving-data.html)

一旦知道如何输入SQL语句，就可以访问数据库了。

假设你在家里有几个宠物（你的食品），你想跟踪各种类型的信息。您可以通过创建表来保存数据并加载所需的信息来实现。然后，您可以通过从表中检索数据来回答关于您的动物的不同类型的问题。本节介绍如何执行以下操作：

* 创建数据库
* 创建表
* 将数据加载到表中
* 以各种方式从表中检索数据
* 使用多个表

menagerie数据库很简单，但是不难想到现实世界中可能使用类似类型的数据库的情况。例如，这样的数据库可以被农民用来跟踪牲畜，或者由兽医来跟踪患者记录。可以从MySQL Web站点获取包含以下部分中使用的一些查询和样本数据的menagerie分发。它提供压缩的tar文件和Zip格式在[http://dev.mysql.com/doc/。](http://dev.mysql.com/doc/。)

使用SHOW语句查找服务器上当前存在哪些数据库：

```
mysql>
SHOW DATABASES;
+----------+
| Database |
+----------+
| mysql    |
| test     |
| tmp      |
+----------+
```

mysql数据库描述用户访问权限。测试数据库通常作为工作区提供给用户尝试。

语句显示的数据库列表可能在您的计算机上不同;如果您没有SHOW DATABASES权限，SHOW DATABASES不显示您没有权限的数据库。请参见第14.7.5.14节“[SHOW DATABASES Syntax](https://dev.mysql.com/doc/refman/5.7/en/show-databases.html)”。

如果测试数据库存在，请尝试访问它：

```
mysql>USE test
Database changed
```

USE和QUIT命令一样，不需要分号。 （如果你喜欢，你可以用分号终止这样的语句;它没有任何危害。）USE语句也是另一种特殊的：该命令不能换行。

您可以使用测试数据库（如果您可以访问它）后面的示例，但您在该数据库中创建的任何内容都可以由任何其他有权限的用户删除。因此，您应该询问您的MySQL管理员是否有权使用您自己的数据库。假设你想调用你的menagerie。管理员需要执行如下语句：

```
mysql>GRANT ALL ON menagerie.* TO 'your_mysql_name'@'your_client_host';
```

其中your\_mysql\_name是分配给您的MySQL用户名，your\_client\_host是您连接到服务器的主机。

