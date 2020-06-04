[TOC]



##  git pull/push 免输入密码

1.使用文件创建用户名和密码

文件创建在用户主目录下：

root用户是 /root/      其他用户是 /home/用户名/

```shell
touch .git-credentials
vim .git-credentials
https://{username}:{password}@github.com
记得在真正输入的时候是没有大括号的。
例子:
https://feng99:mima@github.com
```



2.添加git config内容

```
git config --global credential.helper store
```

执行此命令后，用户主目录下的.gitconfig文件会多了一项：`[credential]`

```
helper = store
```

重新git push就不需要用户名密码了。