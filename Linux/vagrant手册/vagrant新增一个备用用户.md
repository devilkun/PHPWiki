##  不要这么操作!!!  可能会导致 无法重启

此文档废弃

------



官网的box文件   默认用户名是 vagrant

但是,关闭了 密码登陆方式  只能以公钥私钥的方式

以下操作先使用win10自带的cmd  

执行vagrant ssh 先进入虚拟机系统中

https://www.cnblogs.com/ChinaHook/p/6087632.html

### 1.开启密码登陆方式

```
sudo su
vim /etc/ssh/sshd_config
PermitRootLogin prohibit-password
改为
PermitRootLogin yes

PasswordAuthentication no
修改为
PasswordAuthentication yes

重启ssh服务
service sshd restart
```



### 2.添加一个备用账户

在任意目录新建一个 useradd.sh文件

粘贴以下内容    这个例子的用户名/密码为 zhangsan/zhangsan   可根据需求修改

```
#!/bin/bash
set -Ceu

USER="zhangsan"
PASSWORD=$(perl -e 'print crypt("zhangsan", "\$6\$");')
sudo useradd -p ${PASSWORD} -m ${USER}
```

```
执行  sh ./useradd.sh
```

#### 赋予新用户sudo权限



将zhangsan用户添加到 /etc/sudoers中 ,这样就拥有sudo 权限了

1. 添加文件的写权限。也就是输入命令

   ``` chmod u+w /etc/sudoers ```

   

   

2. 编辑/etc/sudoers文件。输入命令
``` vim /etc/sudoers ```,
,进入编辑模式，找到这一 行："
``` root ALL=(ALL) ALL ```
在起下面添加"
``` zhangsan ALL=(ALL) ALL ```
然后保存退出。

此文,参考这个文章
https://www.cnblogs.com/feixiangmanon/p/10992087.html









`config.ssh.private_key_path`
用于SSH进入客户机的私钥的路径