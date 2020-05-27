### 新增一个用户,允许任意地址远程访问

> 只能在本机环境  这么操作  rc pro禁止添加这样的用户

```
进入mysql
mysql -uroot -p
添加一个用户名为db_user，密码为123456，授权为% （%表示所有IP能连接）
对db_test数据库有所有权限
grant all privileges on dbuser.* to db_user@'%' identified by '123456';
flush privileges;
exit;//退出


```

