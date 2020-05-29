

[TOC]



 [PHP 教父鸟哥 Yar 的原理分析](https://www.cnblogs.com/tianshifu/p/7156591.html)

[php.net  YAR的官方文档]( https://www.php.net/manual/zh/book.yar.php)

https://www.phpjieshuo.com/archives/127/

## 安装文档

1.需要安装Msgpack扩展
2.如果需要使用Msgpack作为打包协议, 则需要在configure的时候加上--enable-msgpack
3.扩展包 源码下载地址 https://pecl.php.net/package/yar
### 先安装msgpack扩展
phpize
./configure --with-php-config=/usr/local/php/bin/php-config  
make && make install



### 安装yar扩展

```
wget https://pecl.php.net/get/yar-2.0.7.tgz
tar zxvf 
cd yar-2.0.7
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install

#修改php.ini 添加yar扩展信息

#重启php-fpm 查看phpinfo()信息,是否安装成功

```



 







### php.ini中的配置信息
名字	默认	可修改范围	更新日志
yar.packager	php	PHP_INI_SYSTEM	
yar.debug	Off	PHP_INI_ALL	
yar.connect_timeout	1000	PHP_INI_ALL	
yar.timeout	5000	PHP_INI_ALL	
yar.expose_info	On	PHP_INI_SYS



```
;设置Yar的打包工具, 可以是PHP(serialize), JSON, Msgpack(这个需要编译的时候指定--enable-msgpack).
yar.packager=php
;打开的时候, Yar会把请求过程的日志都打印出来(到stderr).
yar.debug = On
;连接超时(毫秒为单位)
yar.connect_timeout = 2000

;处理超时(毫秒为单位)
yar.timeout = 3000

;如果关闭, 则当通过浏览器访问Server的时候, 不会出现Server Info信息.
yar.expose_info = true

```

### 预定义产量
```
YAR_VERSION (string)
YAR_CLIENT_PROTOCOL_HTTP (integer)
YAR_CLIENT_OPT_PACKAGER (integer)
YAR_CLIENT_OPT_TIMEOUT (integer)
YAR_CLIENT_OPT_CONNECT_TIMEOUT (integer)
YAR_PACKAGER_PHP (string)
YAR_PACKAGER_JSON (string)
YAR_ERR_OKEY (integer)
YAR_ERR_OUTPUT (integer)
YAR_ERR_TRANSPORT (integer)
YAR_ERR_REQUEST (integer)
YAR_ERR_PROTOCOL (integer)
YAR_ERR_PACKAGER (integer)
YAR_ERR_EXCEPTION (integer)
```

### 代码示例

**Yar Server 端示例**

```
<?php

/* 假设这个页面的访问路径是: http://example.com/operator.php */

class Operator {

    /**
     * Add two operands
     * @param interge 
     * @return interge
     */
    public function add($a, $b) {
        return $this->_add($a, $b);
    }

    /**
     * Sub 
     */
    public function sub($a, $b) {
        return $a - $b;
    }

    /**
     * Mul
     */
    public function mul($a, $b) {
        return $a * $b;
    }

    /**
     * Protected methods will not be exposed
     * @param interge 
     * @return interge
     */
    protected function _add($a, $b) {
        return $a + $b;
    }
}

$server = new Yar_Server(new Operator());
$server->handle();
?>
```
通过浏览器访问(GET请求)

以上例程的输出类似于：

![image-20200508231625290](C:\Users\aaa\AppData\Roaming\Typora\typora-user-images\image-20200508231625290.png)

**Yar Client示例**
```
<?php
$client = new yar_client("http://example.com/operator.php");


//下面两个设置为可选  当时建议设置
//设置超时时间为1秒
$client->SetOpt(YAR_OPT_CONNECT_TIMEOUT, 1000);

//Set packager to JSON
//设置包体数据为json各式 
$client->SetOpt(YAR_OPT_PACKAGER, "json");

/* call directly */
var_dump($client->add(1, 2));

/* call via call */
var_dump($client->call("add", array(3, 2)));


/* __add can not be called */
var_dump($client->_add(1, 2));
?>
```
以上例程的输出类似于：
```
int(3)
int(5)
PHP Fatal error:  Uncaught exception 'Yar_Server_Exception' with message 'call to api Operator::_add() failed' in *
```
####  Yar Concurrent Client 并发请求示例
```
<?php
function callback($ret, $callinfo) {
    echo $callinfo['method'] , " result: ", $ret , "\n";
}

/* 注册一个异步调用 */
Yar_Concurrent_Client::call("http://example.com/operator.php", "add", array(1, 2), "callback");
Yar_Concurrent_Client::call("http://example.com/operator.php", "sub", array(2, 1), "callback");
Yar_Concurrent_Client::call("http://example.com/operator.php", "mul", array(2, 2), "callback");

/* 发送所有注册的调用, 等待返回, 返回后Yar会调用callback回掉函数 */
Yar_Concurrent_Client::loop();
?>

以上例程的输出
mul result: 4
sub result: 1
add result: 3

如果想再发起请求,需要重置Call,否则上面的call会再次被调用
/* 重置call ,否则上面的call会调用*/
Yar_Concurrent_Client::reset();
Yar_Concurrent_Client::call("http://example.com/operator.php", "sub", array(2, 1), "callback");
Yar_Concurrent_Client::loop();
```


## $callinfo 包含的属性
1.$callinfo["method"]  被调用的服务函数名称
2.$callinfo["sequence"] 被调用的服务函数 序列   顺序
3.$callinfo["uri"] 被调用的服务的访问地址

## 注意事项


1.php的display_errors与yar.debug不能同时开启

因为:

yar是rpc框架么, 自然有自己的协议, 也就是对输出的信息, 是有格式的. 

如果你输出php默认的错误/调试信息, 那这个格式就错了, 自然就"malformed response header"错误了





2.并行调用

一个页面有多个模块，其中部分模块的数据要依赖其他模块获取的接口数据
所以我就先使用
Yar_Concurrent_Client::call(A)
Yar_Concurrent_Client::call(B)
Yar_Concurrent_Client::loop();
得到数据之后供其他模块接口调用
Yar_Concurrent_Client::call(C)
Yar_Concurrent_Client::call(D)
Yar_Concurrent_Client::loop();

4.必须使用POST请求rpc服务
来自客户端的调用, 都是通过POST请求发送过来的. 如果一个GET请求访问到这个Server, 那在yar.expose_info开启的情况下, 这个服务的Server Info信息会被展现.

3.如果对公网提供http 接口 ,如何保证安全性

请看[微服务架构下的鉴权，怎么做更优雅？](https://learnku.com/articles/30704)