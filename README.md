持续更新,觉得有帮助的欢迎star,希望补充内容的欢迎提issues


## **版权声明:**
> 以下出现的文章,
> 部分是我自己写的,
> 部分是看其他人写的不错, 链接上的,
> 版权归原作者所有.

## PHP
[PHP常用命令](https://www.jianshu.com/p/c871342a7310)

### php extension扩展包安装
先去http://pecl.php.net/ 找到源码下载地址
**第一种方式PECL**
```
pecl install  http://pecl.php.net/get/redis-4.1.0RC2.tgz
```

**第二种方式PHPIZE**
```
1.下载源码包,并解压
2.phpize
3./configure --with-php-config=/usr/local/php/bin/php-config
如果不知道php-config的路径,可以用这个命令查找
find / -name php-config
4.make && make install
```

### 项目依赖包管理
> 类似java的Maven
> [composer教程](https://learnku.com/docs/composer/2018)

## MVC框架
### Phalcon
> C语言编写,以PHP扩展的方式加载,比Yii Laravel性能更快,
> 单独说性能最好,还是鸟哥的Yaf
> 但是Yaf功能太过于简陋,很多组件需要自己写,易用性较差
> 综合而言,phalcon在性能较快的情况下,更兼容了易用性,为上上之选.

[phalcon3.4官方中文文档](https://www.kancloud.cn/jaya1992/phalcon_doc_zh/753240)
## PHP性能分析与监控
### xhprof+xhgui [PHP5.x]
> 注意 xhprof只支持到php5.6  facabook公司不再维护
> php7以后的xhprof,更换了名字叫tideways 

[php-monitor](https://github.com/laynefyc/php-monitor/blob/master/README-zh_CN.md)

### Tideways+xhgui [PHP7.x]

[PHP7开启opcache打造强悍性能](https://www.cnblogs.com/lamp01/p/8985068.html)

[Swoole整合PHP性能监控分析平台: Tideways+Xhgui](https://blog.csdn.net/uisoul/article/details/103936873)




## Swoole
### 基于swoole的框架
#### easySwoole 
>简称 esw
>[EasySwoole官方文档](https://www.easyswoole.com/Cn/Preface/introduction.html)
## Nginx

## Mysql

## Redis
[Redis-Cli全部命令](https://www.runoob.com/redis/redis-commands.html)

redis的数据结构及使用场景
缓存穿透问题
缓存雪崩问题
缓存热点Key的并发问题
redis持久化方案对比
redis集群方案 


## ElasticSearch
> Es主要解决大文本搜索  取代mysql的like低效查询

倒排索引的原理
### Mysql与ElasticSearch数据实时同步的工具
[mysqlsmom](https://github.com/m358807551/mysqlsmom)
[mysqlsmom的中文教程](https://mysqlsmom.readthedocs.io/en/latest/)

[go-mysql-elasticsearch](https://github.com/siddontang/go-mysql-elasticsearch)





## 消息队列
##### [消息队列应用场景](https://www.kancloud.cn/vson/php-message-queue/885555)
##### [使用消息队列的注意事项](https://www.kancloud.cn/vson/php-message-queue/885556)



### beanstalk
常用命令
> 查看状态`service beanstalkd status`
> 配置文件地址 `/etc/sysconfig/beanstalkd`
> 或者`/etc/default/beanstalkd`
> 启动  `service beanstalkd start`
> 停止  `service beanstalkd stop`
> 重启  `service beanstalkd restart`

[beanstalk队列教程](https://www.kancloud.cn/vson/php-message-queue/891903)

[beanstalk的php客户端phanstalk](https://github.com/pheanstalk/pheanstalk)

[beanstalkWeb管理工具](https://github.com/ptrofimov/beanstalk_console)
> PHP语言写的   和下面那个功能一样
> 下载代码包,配置下nginx,就可以访问. 
> beanstalk信息在config.php中配置
> ![image.png](/img/bVbGLZr)

[基于 Web 的 Beanstalk 消息队列服务器管理工具](https://github.com/xuri/aurora)
> Go语言写的
> 1.查看有哪些tube
> 2.tube里 待消费任务数量  延迟任务数量 看是否有队列任务阻塞 
> 3.支持 Job 模糊搜索
> 4.支持清空某个tube里所有的任务

### RabbitMQ
[轻松搞定RabbitMQ](https://www.kancloud.cn/longxuan/rabbitmq-arron/117512)


## 代码部署
### Walle
[Walle官方网站](https://www.walle-web.io/)
[Walle GitHub地址](https://github.com/meolu/walle-web)
> 建议直接使用最新版 从官网下载即可
> 1.支持代码部署后通知到邮件/钉钉群  钉钉群这个很实用
> 2.支持部署后,立刻回滚.
### Jenkins

## 项目管理工具
### 代码版本管理 
#### gitlab

#### gogs
> 比gitlab更清爽些
> https://gogs.io/


### wiki 
#### confluence
>  用过都说好 没有比这个更好的
>  1.程序设计文档模板
>  2.数据库设计文档模板
>  3.接口设计文档模板(可以不要,由YAPI替代.)
>  **4.缺点 要求机器至少6GB内存  **
>  [confluence 安装及破解](https://www.qinjj.tech/2019/01/04/confluence%20install/)


#### mindoc
[mindoc使用文档](https://www.iminho.me/wiki/docs/mindoc/mindoc-linux.md)

> mm-wiki 和 mindoc类似  
> 推荐使用mindoc

#### mindoc
[mindoc使用文档](https://www.iminho.me/wiki/docs/mindoc/mindoc-linux.md)
#### mm-wiki
https://github.com/phachon/mm-wiki


### api doc
> 类似的接口文档管理工具有十来个吧,用过5个左右,然后发现这个功能和界面最舒服. 也支持mock数据
> [YAPI 去哪玩开源的  点击看教程 ](https://hellosean1025.github.io/yapi/)
### bug管理 禅道
### 代码编辑器phpStorm
>团队内代码编辑器及版本必须强制统一

[phpStorm汉化教程](https://github.com/pingfangx/TranslatorX)
phpStorm激活到2089年教程
[phpStrom自动展开use内容](https://www.coder.work/article/725180)
#### 一定要安装的插件
> java的idea编辑器 比phpstrom的插件更丰富.
> phpstrom真没啥好插件..我只安装了下面这一个
1. Key Promoter X
> 可自定义修改所有的快捷键



## LNMP环境
### vagrant虚拟机
>以下几篇文章是我的CSDN写的,但是后来CSDN的排版混乱不堪,就转向印象笔记了.

[1.Vagrant在，win7/win10系统下搭建使用](https://blog.csdn.net/ifeng6/article/details/75071800)

[2.解决vagrant未开启系统虚拟化错误.virtualization VT-x/AMD-V](https://blog.csdn.net/ifeng6/article/details/75074010)

[3.vagrant之核心配置文件Vagrantfile讲解](https://blog.csdn.net/ifeng6/article/details/75098195)

[4.解决vagrant共享文件夹挂载失败问题.Vagrant was unable to mount VirtualBox shared folders](https://blog.csdn.net/ifeng6/article/details/76316991)

5.[打包自己的vagrant虚拟机,分析给他人使用](https://www.jianshu.com/p/05197ab5b767)
> 1.假如你有2台电脑(笔记本/台式机) 开发环境要做的一模一样,A电脑配置好后,打包   B电脑导入即可.
> 2.技术部,所有后端人员的本机开发环境要求统一,也可以使用这种方式.


### Lnmp搭建
[配置LNMPHP7.3+phalcon3.4+swoole4.5环境](https://segmentfault.com/a/1190000022527939)

[我常用的Linux命令](https://segmentfault.com/a/1190000022533138)

[CentOS安装MySQL详解](https://segmentfault.com/a/1190000019507071)


### Docker
> 完全可以取代vagrant 
> vagrant虚拟机是一个完整系统
> docker容器是进程级别的
> 启动更快  空间/内存占用更少  安装命令更简单
> 所有软件都可以docker安装 随时删除  宿主机 完全干净
> 镜像比作模板
> 容器比作实例
> 自定义镜像继承官方镜像


 **Docker 的主要用途，目前有三大类。**

**（1）提供一次性的环境。**
比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。

**（2）提供弹性的云服务。**
因为 Docker 容器可以随开随关，很适合动态扩容和缩容。

**（3）组建微服务架构。**
通过多个容器，一台机器可以跑多个服务，因此在本地环境就可以模拟出微服务架构。

[阮一峰大神的docker入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

[docker安装mysql](https://blog.csdn.net/tyt_XiaoTao/article/details/84621087)

[docker安装nginx](https://zhuanlan.zhihu.com/p/60709123)


### 进程管理工具Supervisor
> 用来启动,停止任意进程,(和编程语言无关)

[进程管理工具 Supervisor 使用教程](https://www.restran.net/2015/10/04/supervisord-tutorial/)


## 系统架构
[100W日活 5KW PV的系统架构该如何设计?](https://segmentfault.com/a/1190000022538441)

## 系统设计(业务场景设计)
1.抢红包如何设计
[漫画：如何实现抢红包算法？](https://juejin.im/post/5af80310f265da0b8636585e)
[PHP实现的红包生成器](https://juejin.im/post/5a97c863518825555d46a7e2)
2.滴滴打车系统如何设计
3.Mysql自己实现分词功能 不适用ElasticSearch


## 性能优化专题
### 性能优化之慢日志
#### php-fpm慢日志
#### mysql慢日志
#### redis慢日志
#### php程序运行日志 SeasLog
>1.高并发下日志不会成为性能瓶颈 秒QPS 8000
>2.自带日志分析预警框架(默认关闭)(主要用来分析Error日志)
>A:统计某种日志级别的数量  
>B:查看某种日志级别的所有日志
>C:通过邮件方式发出日志报警
>3.支持每log N次后,日志数据写入磁盘(日志缓冲区)
>4.支持RequestId 请求全局id区分请求
>5.支持按照时间切割日志

[SeasLog官方文档](https://github.com/SeasX/SeasLog/blob/master/README_zh.md)


[性能优化攻略](https://www.kancloud.cn/lw198962/codeperformance/107454)

### 性能优化之数据库Mysql
合理设置最大连接数(max_connections)
Explain查询分析器
Mysql高可用-主从配置
#### mysql数据库连接池

### 性能优化之PHP代码逻辑优化
使用队列进行流量削峰

同步阻塞请求修改为异步非阻塞

## 运维监控
### open-falcon
> 由小米公司研发并开源
> 小米/美团/京东/滴滴等大公司都在使用
> 目前 业内 最强大的运维监控软件
> 支持进程端口/Mysql/Redis/Memcache/Nginx/硬件/URL等等监控.

[官网http://open-falcon.com/](http://open-falcon.com/)

[V2.0官方中文教程](http://book.open-falcon.com/zh_0_2/intro/)

## 设计模式

[PHP设计模式教程](https://www.awaimai.com/patterns)
[PHP设计模式全集](https://learnku.com/docs/php-design-patterns/2018)

## 数据结构

## 算法

## 待整理,因为我还没有看过
> 本人没看过的,绝对不能放到上面 整理不是最终目的 学习才是目的 做收集者 是没用的

API设计指南
https://www.kancloud.cn/special/api
Linux/Unix学习篇
https://www.kancloud.cn/special/linux
Redis高性能key-value存储系统
https://www.kancloud.cn/special/redis
HTTP和那些互联网协议
https://www.kancloud.cn/special/http