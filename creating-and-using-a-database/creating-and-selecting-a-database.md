### 4.3.1 Creating and Selecting a Database

如果管理员在创建数据库，为您设置了权限。您可以开始使用它。否则，你需要自己创建：

```
mysql>CREATE DATABASE menagerie;
```

在UNIX，数据库名称是区分大小写的（不像SQL关键字）。（在Windows的领导下，该限制不适用，虽然你要参考数据库和表使用相同的lettercase在一个给定的查询。然而，由于种种原因，推荐的最佳实践是使用相同的lettercase被用来当数据库被创建。）

**注：如果你得到一个错误，如错误1044（42000）：用户访问被拒绝”micah'@'localhost'数据库'menagerie'在尝试创建一个数据库，这意味着，您的用户帐户没有这样做的必要的权限。以管理员或见 7.2章节： Section 7.2, “The MySQL Access Privilege System”。**

创建数据库不会默认选择使用它，必须显式地执行它。把menagerie设置为当前数据库，使用此语句：

```
mysql>USE menagerie
Database changed
```

您的数据库只需要创建一次，但您必须在每次MySQL会话开始时选择它。您可以通过一个命令语句，如示例中所示。或者，您可以在调用MySQL时选择命令行上的数据库。只需在您可能需要提供的连接参数后指定其名称。例如:

```
shell>mysql -h host -u user -p menagerie
Enter password: ********
```

**注：menagerie只是在命令行中显示，不是密码。如果你的-p选项后要提供您的密码在命令行中，你必须这样做，（例如，作为 -pmypassword，不是-P mypassword）。然而，在命令行上输入密码是不被推荐的，因为这样做会使它暴露在你的机器上登录的其他用户的窥探中。**

**注：你可以在任何时间使用 SELECT DATABASE\(\)命令查看正在使用的数据库。**

