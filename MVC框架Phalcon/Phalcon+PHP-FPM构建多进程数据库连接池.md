## 原文连接

http://www.jicker.cn/5641.html/comment-page-2?unapproved=61309&moderation-hash=798a205dd80376f72cefbd35eaa06267#comment-61309



## 长连接不适用的场景

```
由于脚本语言的特殊性，PHP官方不建议在事务操作或者可能引起锁表的情况下，使用PHP的持久化数据库连接。因为会导致数据库锁死
```



## 代码

```
// Create a connection with PDO options
$connection = new \Phalcon\Db\Adapter\Pdo\Mysql(
    [
        "host"     => "localhost",
        "username" => "root",
        "password" => "sigma",
        "dbname"   => "test_db",
        "options"  => [ //这里加上此附加参数
            PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES 'UTF8'",
            PDO::ATTR_PERSISTENT => true,//长连接
        ]
    ]
);
```

