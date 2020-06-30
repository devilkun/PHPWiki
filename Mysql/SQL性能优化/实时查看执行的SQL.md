## 注意! 只允许在开发/测试环境开启  因为很占用磁盘空间

MySQL默认不能实时查看执行的SQL语句，因为这会消耗一定的资源。

要开启这个功能，稍微配置一下，打开这个LOG记录就可以了。

## 1 查看LOG功能

首先，查看是否已经开启实时SQL语句记录。

```
mysql> SHOW VARIABLES LIKE "general_log%";
```

如下`general_log`值为`OFF`

```
+------------------+----------------------------------+
| Variable_name    | Value                            |
+------------------+----------------------------------+
| general_log      | OFF                              |
| general_log_file | /var/lib/mysql/galley-pc.log |

```

## 2 打开LOG功能

### 2.1 临时开启

如下，打开实时记录SQL语句功能，并指定自定义的log路径：

```php
mysql> SET GLOBAL general_log = 'ON';



mysql> SET GLOBAL general_log_file = '/var/log/mysql/general_log.log';
```

这两个命令在MySQL重启后失效，为临时方法。

**说明：这个文件会随着访问的增加而不断变大，所以生产环境建议临时开启，用完及时关闭。**

## 2.2 永久开启

永久有效需要配置 **my.cnf文件**，加入下面两行：

```
general_log = 1
general_log_file = /var/log/mysql/general_sql.log
```

重启MySQL生效。

## 3 实时查看

```
tail -f /var/log/mysql/general_sql.log
```

