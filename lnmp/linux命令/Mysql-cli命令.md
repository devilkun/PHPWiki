### 新增一个用户,允许任意地址远程访问

> 只能在本机环境  这么操作  rc pro禁止添加这样的用户

```
进入mysql
/usr/local/mysql/bin/mysql -uroot -p
mysql -uroot -p
添加一个用户名为db_user，密码为123456，授权为% （%表示所有IP能连接）
对db_test数据库有所有权限
下面这个sql语句 执行第二次  会覆盖第一次的密码
grant all privileges on dbstatis.* to db_user@'%' identified by 'miai888gtopphp';
flush privileges;
exit;//退出


```



### 创建数据库

```
CREATE DATABASE IF NOT EXISTS dbstatis  DEFAULT CHARACTER SET utf8;
```



### MySQL报错：[Err] 1055 - Expression #1 of ORDER BY clause is not in GROUP BY clause

```
select version(),
@@sql_mode;SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
```

### 临时  用完删除

```
grant all privileges on dbcmsucenter.* to db_user@'%' identified by 'miai888gtopphp';
flush privileges;
```

