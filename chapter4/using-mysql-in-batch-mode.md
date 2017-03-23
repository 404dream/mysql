## 4.5 Using mysql in Batch Mode

在前面的部分中，都是以交互方式使用mysql输入语句并查看结果。您也可以在批处理模式下运行mysql。为此，将要运行的语句放在一个文件中，然后告诉mysql从文件中读取其输入：

```
shell>mysql < batch-file
```

如果您在Windows下运行mysql，为了避免特殊字符带来的问题，您可以这样做：

```
C:\>mysql -e "source batch-file"
```

如果需要在命令行中指定连接参数，命令可以如下所示：

```
shell>mysql -h host -u user -p < batch-file
Enter password: ********
```

当您以这种方式使用mysql时，您需要先创建一个脚本文件，然后执行该脚本。

如果脚本有一些语句产生错误仍然需要执行，则应使用--force命令行选项。

为什么要使用脚本？以下是几个原因：

* 如果您反复（例如每天或每周）运行查询，使其成为脚本，则可以在每次执行时避免重新输入。
* 您可以通过复制和编辑脚本文件来生成与现有查询类似的新查询。
* 在编写查询语句时，批处理模式也是有用的，特别是对于多行语句或多语句序列。如果你犯了一个错误，你不需要重新输入一切。只需编辑您的脚本来纠正错误，然后告诉mysql再次执行它。
* 如果查询结果很多，您可以分页输出，而不是看着它从屏幕顶部滚动：

```
shell>mysql <batch-file | more
```

* 您可以捕获文件中的输出进行进一步处理：

```
shell>mysql < batch-file > mysql.out
```

* 您可以将脚本分发给其他人，以便他们也可以运行语句。

* 某些情况不允许进行交互式使用，例如，当您从cron作业运行查询时。在这种情况下，您必须使用批处理模式。

当您以批处理模式运行mysql时，默认输出格式与使用交互式运行时不同（更简洁）。例如，当交互式运行mysql时，SELECT DISTINCT species FROM pet的输出看起来像这样：

```
+---------+
| species |
+---------+
| bird    |
| cat     |
| dog     |
| hamster |
| snake   |
+---------+
```

在批处理模式下，输出如下所示：

```
species
bird
cat
dog
hamster
snake
```

如果要以批处理方式获取交互式输出格式，请使用mysql  -t。要输出执行的语句输出，请使用mysql  -v。

您还可以使用来自mysql提示符的脚本使用source命令或\ . 命令：

```
mysql>source filename;
mysql>\. filename
```

有关详细信息，请参见第5.5.1.5节“从文本文件执行SQL语句”。

