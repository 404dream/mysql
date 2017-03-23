## Connecting to and Disconnecting from the Server

当你连接到服务器调用mysql时，你通常需要提供一个MySQL的用户名和密码。如果服务器在您登录的计算机上运行，则还需要指定主机名。请与您的管理员联系以了解您应该使用的连接参数（即，要使用的主机、用户名和密码）。一旦你知道了适当的参数，你应该能够像这样连接：

```
shell>mysql -h host -u user -p
Enter password: ********
```

host和user代表你的MySQL服务器运行时的主机名和你的MySQL帐户的用户名。为您的设置替换适当的值。\*\*\*\*\*\*\*\*代表您的密码，当终端显示Enter password时输入。

如果成功，你会看到一些入门提示，其次是进入MySQL的命令行，告诉用户随时可以输入SQL语句。

```
shell>mysql -h host -u user -p
Enter password: ********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 25338 to server version: 5.7.19-standard
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.
mysql>
```

如果你登录到的MySQL运行在同一台机器，你可以省略主机，简单地使用下面的：

```
shell>mysql -u user -p
```

如果当您尝试登录，你得到一个错误信息，如ERROR 2002（HY000）：不能通过socket '/temp/mysql.sock'连接到本地MySql服务\(2\)，这意味着MySQL服务器守护进程（UNIX）或服务（Windows）没有运行。咨询管理员或回顾第二章：[Installing and Upgrading MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html)，使之适合您的操作系统。

帮助试图登录时经常遇到的其他问题：[Section B.5.2, “Common Errors When Using MySQL Programs”](https://dev.mysql.com/doc/refman/5.7/en/common-errors.html)

一些MySQL的安装允许用户为匿名连接（未命名）用户的服务器在本地主机上运行。如果这是你的机器的情况下，你应该能够连接到通过调用MySQL没有任何选项：

```
shell>mysql
```

在你已经连接成功，你可以在任何时间在MySQL 命令行中通过键入 QUIT（或\q）断开连接：

```
mysql>QUIT
Bye
```

在UNIX上，您也可以通过按Ctrl + D断开。

以下几节中的大多数实例假定您连接到服务器。它们都是在MySql命令行下操作。

