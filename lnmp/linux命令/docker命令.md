## Docker命令
**安装mysql**
```
docker run -p 3306:3306 --name wiki-mysql -v /mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456  -d mysql:5.7

参数说明：

-name：指定容器名  这个例子的是 wiki-mysql

-p：映射宿主主机端口

-v：挂载宿主目录到容器目录

-e：设置环境变量，此处指定root密码

-d：后台运行容器


进入docker镜像
docker exec -it wiki-mysql /bin/bash

访问mysql
mysql -h172.17.0.1 -p3306 -uroot -p123456

修改mysql访问权限 修改root 可以通过任何客户端连接
ALTER  USER  'root'@'%'  IDENTIFIED  WITH mysql_native_password BY  '123456';
```
** 查看正在运行的docker容器实例**
```
以下两个命令都可以,输出的结果是一样的
docker ps

docker container ls

```
**立刻停止指定的容器运行**
```
 docker container kill [containerID]
 dicker kill [containerID]
```
**容器停止运行之后，物理文件还在，用下面的命令删除容器文件。
删除指定的容器文件**
```
docker container rm [containerID]
docker rm [containerID]
```

**启动/停止 指定的容器实例**
```
docker container start [containerID]

docker container stop [containerID]
```
**查看指定容器实例的日志信息**
```
docker container logs [containerID]
```

**进入某个正在运行的容器实例内部**
```
docker exec -it [containerID] /bin/bash
docker container exec -it [containerID] /bin/bash
```

**从正在运行的 Docker 容器里面，将文件拷贝到本机。**
**下面是拷贝到当前目录的写法。**

```
docker cp [containID]:[/path/to/file] .
```

 修改ll和ls -l命令中显示的日期格式/时间格式 年-月-日 时-分-秒
 ```
 	1、临时更改显示样式，当回话结束后恢复原来的样式
    export TIME_STYLE='+%Y-%m-%d %H:%M:%S'    # 直接在命令中执行即可

	2、永久改变显示样式，更改后的效果会保存下来
    修改/etc/profile文件，在文件内容末尾加入
    export TIME_STYLE='+%Y-%m-%d %H:%M:%S'

    执行如下命令，使你修改后的/etc/profile文件配置内容生效
    source /etc/profile
 ```