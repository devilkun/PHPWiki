
### **实时查看日志**

```
tail -f 日志名称
```

### **以json方式查看日志**

> 需要安装jq软件
```
tail -f test.log | jq
```

**统计目录内文件总大小**

```
du -sh .
```

### **统计目录内文件大小  按照文件从大到小排序**

```
du -sh * | sort -nr
```



### **查看磁盘已用/可用空间**

```
df -h
```

### **查看某个端口是否在使用**

```
netstat -an | grep '9500'
```



### **查看linux哪些端口允许被访问**

iptables -L -n --line-number
ACCEPT 表示允许

### **查看某个程序是否在运行**

```
ps -ef | grep php-fpm
```

### **统计某个程序的运行数量**

```
ps -ef | grep php-fpm | wc -l
```

### 上传单个文件

```
rz 选择一个文件
```
### **下载单个文件**

```
sz 文件名称
```

### **修改ll和ls -l命令中显示的日期格式/时间格式 年-月-日 时-分-秒**

 ```
 	1、临时更改显示样式，当回话结束后恢复原来的样式
    export TIME_STYLE='+%Y-%m-%d %H:%M:%S'    # 直接在命令中执行即可

	2、永久改变显示样式，更改后的效果会保存下来
    修改/etc/profile文件，在文件内容末尾加入
    export TIME_STYLE='+%Y-%m-%d %H:%M:%S'

    执行如下命令，使你修改后的/etc/profile文件配置内容生效
    source /etc/profile
 ```

### 修改目录的所属用户和用户组

```
chown [-R] 账号名称:用户组名称 文件或目录
示例  把/data/logs的权限交给nginx的www用户
用户组 必须再/etc/group里存在
chown -R www:www /data/logs/ 
以下命令可选
find /data/wwwroot/ -type d -exec chmod 755 {} \;
find /data/wwwroot/ -type f -exec chmod 644 {} \;
```



