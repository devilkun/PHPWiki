[TOC]



# linux安装SVN

```
apt-get install subversion
或者
yum install subversion


```

# 创建SVN仓库

```
svnadmin create /root/svn/doc
doc就是仓库的名字
```



# 启动SVN服务端

```
svnserve -d -r /root/svn/
```

# 配置用户分组 用户名 密码

```
vim passwd
vim svnserve.conf
vim authz

```

# 阿里云机器 开启svn 3690端口

# win10安装SVN客户端

```
去svn官网下载吧
TortoiseSVN-1.9.4.27285-x64-svn-1.9.4.msi
中文语言包
LanguagePack_1.9.4.27285-x64-zh_CN.msi

```



```
svn://ip地址/doc
```



# 遇到的问题

## 1.停止已经启动的svn server

```
没有svn stop 这样的命令

使用以下方式
ps -ef|grep svn
找到进程ID

kill PID 就可以了
```



## 2.checkout时，提示：URL svn://xx/svntest doesn't exist

```
如果你的svn库的路径为：/www/svn/think(这是你版本库的路径，就是你Linux上的仓库)

那么你启动时，不能用命令：

svnserve -d -r /home/svn/think
而要用命令：

svnserve -d -r /home/svn/
```

## 3.commit时，提示：Authorization failed

**svnserve.conf**

```
# anon-access = read 
# auth-access = write 
# password-db = passwd 
# authz-db = authz

修改为
anon-access = read 
auth-access = write 
password-db = passwd 
authz-db = authz
```



## 4.svn: line 19: Option expected

```
修改svnserve.conf时，打开注释时，配置的前面有空格，把最前面的空格删掉
```





## 5.commit时，提示：Invalid authz configuration

> 注意: 这个提示 和第3个问题  不是同一个问题 

主要是auth文件没写对

以下为正确的配置  注意admin前面的@符号



```
[groups]
admin = liuaifeng
dev = zhangsan

[/]
# [/foo/bar]
@admin = rw
@dev = r
# * =

[repository:/doc]
@admin = rw
* = r

```

## 6.修改配置文件后不生效   答案:必须重新svn server

# SVN常用命令

```
检查authz文件是否正确
svnauthz-validate /root/svn/doc/conf/authz

启动SVN的服务(-d:Deamon; -r:Root)
svnserve -d -r /root/svn/


查看SVN的服务是否正常（端口号3690是否存在）
netstat -ntlp
```





