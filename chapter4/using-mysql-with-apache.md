## 4.7 Using MySQL with Apache

有一些程序会让您通过MySQL数据库验证用户，还可以将日志文件写入MySQL表。

您可以通过将以下内容放入Apache配置文件中使Apache日志记录格式更改为易于读取：

```

```

```
LogFormat \
        "\"%h\",%{%Y%m%d%H%M%S}t,%>s,\"%b\",\"%{Content-Type}o\",  \
        \"%U\",\"%{Referer}i\",\"%{User-Agent}i\""
```

要将该格式的日志文件加载到MySQL中，可以使用如下语句：

```
LOAD DATA INFILE '/local/access_log' INTO TABLE tbl_name
FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' ESCAPED BY '\\'
```

应创建一个具有与LogFormat日志文件的列对应的表。

