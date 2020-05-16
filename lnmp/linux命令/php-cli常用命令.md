[TOC]

### CLI 与 CGI 的区别

CLI 是 PHP 专门提供给我们与命令行交互的工具。CGI 是 PHP 提供给 Web Server 通信的接口。所以，在我们的 PHP 项目开发当中，很多框架都会提供一个 Web 模式的入口，一个 CLI 的入口。

- 与 CGI 不同，CLI 其输出没有任何头（Header）信息。
- CLI 模式下，出错时输出纯文本的错误信息（非 HTML 格式）。
- CLI 模式下，所有来自 print 和 echo 的输出将被立即写到输出端。而不作任何缓冲操作。
- CLI 模式下，最大运行时间（`max_execution_time`）被设置为无限值。
- CLI 模式下，`$argc` 与 `$argv` 两个变量总是存在。并且携带了参数个数与实际的参数数组值。
- *CLI SAPI* *不会*将当前目录改为已运行的脚本所在的目录。
- CLI 模式提供了几个专用常量：STDIN、STDOUT、STDERR。

> 注意：配置文件（php.ini）当中 html_errors、implicit_flush、max_execution_time、register_argc_argv 对其修改对 CLI 模式下运行的 PHP 来说没有任意作用。因为，CLI 启动的时候会重置这些值。

接下来我们对它们的这些区别进行分开说明。

**2.1）CLI 模式没有任何头（Header）信息。**

因其 CLI 是命令行下交互的接口。Header 头是 HTTP 协议定义的一部分。在 CGI 模式才支持这些交互协议。所以，Header 头信息对 CLI 模式来说并没有任何意义。所以，CLI 模式不会有任何头信息。PHP 也没有提供相应的配置来开启支持头信息的输出。

**2.2）CLI 模式常用的输出会立即写到输出端，而不作任何缓冲操作。**

在 CGI 模式下，因其要考虑到性能以及通信的问题。所以，服务器端的输出是一次性执行结束输出到客户端。但是， CLI 模式下通常不会有这种的需求。

如果希望延缓或控制标准输出，仍然可以使用 `output buffering` 设置项改变其行为。



**CLI 模式下，`$argc` 与 `$argv` 两个变量总是存在。并且携带了参数个数与实际的参数数组值。**

这两个值是 CLI 模式下专属的值。在 CLI 启动的时候这两个变量就已经被初始化。`$argc` 保存的是当前命令行的参数个数，`$argv` 保存的是命令的参数值，且类型为数组。

所以，在 CLI 开发时，不要对这两个变量进行任何的写操作。除非你明确知道自己这样做的后果。



### 查看 PHP 版本

php -v

### 查看 PHP 扩展列表

php -m

### 查看指定配置信息

php -i|grep session



### 查看 PHP 使用的php.ini配置文件

php --ini



### 查看扩展是否存在



 php --re yafs

 php -m|grep yaf

###  查看扩展配置信息  扩展版本信息

 php --ri yaf





### 刷新Opcache缓存

Reset

http://192.168.33.22/ocp.php