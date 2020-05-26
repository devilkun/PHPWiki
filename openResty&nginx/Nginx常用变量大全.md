### 配合LUA脚本使用

[TOC]





### [与request有关的变量](http://timd.cn/nginx/varindex/#request-about-1)

###### 1，$arg_name

请求行中，名称为*name*的参数的值。比如，当请求行是"GET /nginx/varindex/?from=rss HTTP/1.1"时，$arg_from的值是"rss"。当请求行中没有名称为*name*的参数时，$arg_*name*的值是空字符串

###### 2，$is_args

如果请求行中包含参数，那么$is_args的值是"?"，否则是空字符串

###### 3，$args、$query_string

请求行中的全部参数（也就是查询字符串）。比如，当请求行是"GET /nginx/varindex/?a=b&c=d HTTP/1.1"时，$args的值是a=b&c=d。当请求行中没有任何参数时，$args的值是空字符串

###### 4，$cookie_*name*

名称为*name*的cookie的值

###### 5，$request

完整的原始的请求行。比如："GET /b/../a?a=b HTTP/1.1"（uri不会被规范化）

###### 6，$http_*name*

用来获取任意请求头的值。
HTTP header的命名方式是：每个单词的首字母大写，其余字母小写，单词之间用中划线（"-"）连接。比如，User-Agent、Content-Type、X-Forwarded-For等。
请求头名称和*name*之间的转换方式是：将请求头名称转换成小写形式，并将中划线（"-"）替换成下划线（"_"），比如：$http_x_forwarded_for

###### 7，$request_length

请求的长度，包含请求行、请求头和请求体

###### 8，$request_method

请求方法，比如GET或POST

###### 9，$request_uri  完整的原始的URI（带参数）

###### 10，$scheme  请求模式，http或https

###### 11，$content_length  "Content-Length"请求头的值

###### 12，$content_type  "Content-Type"请求头的值

###### 13，$document_root

应用于当前请求的root或alias指令的值

###### 14，$document_uri、$uri

**规范化后的URI。所谓的规范化是指：将 以"%XX"形式编码的文本进行解码 之后，解决对相对地址"."和".."的引用，并将多个相邻的"/"压缩成一个。
请求处理期间，$uri的值可能发生改变，比如在发生内部重定向或者使用index文件的时候**

###### 15，$host

Nginx按照下面的优先级顺序，设置$host的值：

- 从请求行中获取到的主机名
- 从"Host"请求头中获取到的主机名
- 处理请求的虚拟主机的名称

一般情况下，请求行中只会包含 Request URI ，也就是 URI 和 QUERY STRING ，不会包含 host name。但是，当使用Nginx做HTTP正向代理服务器的时候，请求行中会包含host name

###### 16，$proxy_add_x_fowarded_for

**在客户端传递来的X-Forwarded-For请求头后面追加$remote_addr（用逗号分隔）。如果客户端没有传递X-Forwarded-For请求头，那么该变量等于$remote_addr。
比如，客户端传递来的XFF请求头是：192.168.1.1, 192.168.1.2；$remote_addr是192.168.1.3。那么$proxy_add_x_forwarded_for等于192.168.1.1, 192.168.1.2, 192.168.1.3**

###### 17，$realpath_root

应用于当前请求的root或alias指令的值所对应的绝对路径，所有的符号连接都会被解析成真实路径

###### 18，$server_protocol

请求协议，通常是"HTTP/1.0"、"HTTP/1.1"、"HTTP/2.0"

###### 19，$request_filename

当前请求对应的资源的路径。
该变量的值是基于root（或alias）指令的值，以及请求的URI计算出来的。比如，当root指令的值是/data/w3，URI是/images/logo.jpg时，该变量等于/data/w3/images/logo.jpg

------

#### [与response有关的变量](http://timd.cn/nginx/varindex/#response-about-1)

###### 1，$body_bytes_sent

下发给客户端的字节数（不包含响应头）

###### 2，$request_time

从 接收到请求的第一个字节 到 把响应的最后一个字节发送给客户端，所经历的时间。单位是秒，精确到毫秒

###### 3，$send_http_*name*

用于获取下发给客户端的任意响应头的值。响应头名称和*name*之间的转换方式是：将响应头名称转换成小写形式，并将中划线（"-"）替换成下划线（"_"）

###### 4，$status

下发给客户端的响应码

------

### [常用变量](http://timd.cn/nginx/varindex/#common-variables-1)

###### 1，$binary_remote_addr

二进制形式的客户端地址。对于IPV4地址，该值的长度是4字节，对于IPV6地址该值的长度是16字节

###### 2，$connection

连接的序号

###### 3，$connection_requests

在开启keepalive的情况下，客户端可以使用一条连接发起多个请求，该变量用于记录通过一条连接发起的请求数

###### 4，$date_local

本地时区的当前时间

###### 5，$date_gmt

GMT格式的当前时间

###### 6，$hostname

运行Nginx的主机的主机名，比如：vm*0*16_centos

###### 7，$msec

当前的时间戳，精确到毫秒。比如，1551609371.088

###### 8，$nginx_version

Nginx的版本号

###### 9，$pid

Worker进程的PID

###### 10，$proxy_host

在proxy_pass指令中指定的被代理服务的名称（可能是upstream的名称）

###### 11，$proxy_port

在proxy_pass指令中指定的被代理服务的端口。如果在proxy_pass指令中未指定端口，那么该变量等于协议的默认端口

###### 12，$remote_addr

客户端地址

###### 13，$remote_port

客户端端口

###### 14，$remote_user

在开启basic authentication的时候，客户端所使用的用户名

###### 15，$server_addr

接收请求的虚拟主机的地址

###### 16，$server_port

接收请求的虚拟主机的端口

###### 17，$server_name

接收请求的虚拟主机的名称

###### 18，$time_iso8601

ISO 8601标准格式的本地时间

###### 19，$time_local

通用日志格式的本地时间

------

### [与upstream相关的变量](http://timd.cn/nginx/varindex/#upstream-about-1)

###### 1，$upstream_addr

保存upstream server的ip地址和端口，或Unix套接字的路径。在请求处理期间，如果请求被代理到多个upstream server，那么它们的地址之间用","分隔，比如："192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"。当发生（由error_page等发起的）从一个server group到另外一个server group的内部重定向时，不同的server group之间用":"分隔，比如："192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock : 192.168.10.1:80, 192.168.10.2:80"。如果server group中没有可用的server，那么该变量的值是server group的名字

###### 2，$upstream_bytes_received

从upstream server接收到的字节数。来自于多个连接的值，像$upstream_addr中的地址一样，用","和":"分隔

###### 3，$upstream_bytes_sent（Nginx 1.15.8 起 可用）

发送到upstream server的字节数。来自多个连接的值，像$upstream_addr中的地址一样，用","和":"分隔

###### 4，$upstream_cache_status

该变量用来显示缓存的使用情况。其值是：

- HIT：命中缓存

- MISS：未命中缓存

- EXPIRED：缓存已过期

- BYPASS：客户端强制不使用缓存

- STALE：使用了过期的缓存。可以通过proxy_cache_use_stale指令，指定在什么情况下，使用过期的缓存，比如http_502、error、timeout等

- UPDATING：缓存正在更新

- REVALIDATE：缓存重新生效

  在启用客户端缓存的情况下

  ，服务端可以通过以下两种方式，来判断客户端的缓存是否失效：

  - ETag响应头 和 If-None-Match请求头
  - Last-Modified响应头 和 If-Modified-Since请求头

  当客户端访问一个URL的时候，如果客户端没有缓存或缓存失效，那么服务端会下发资源，以及Last-Modified（资源的最后修改时间）和ETag（Entity Tag，服务端计算的资源指纹，当资源发生改变时，ETag也会改变）响应头。

  以后，客户端再访问该URL时，会带上If-Modified-Since和If-None-Match请求头。其中，If-Modified-Since的值是上次返回的响应中的Last-Modified头的值，If-None-Match的值是上次返回的响应中的ETag头的值。

  下面是Nginx的处理流程，与别的文档的描述不同，Nginx的相关源码在：[github](https://github.com/nginx/nginx/blob/master/src/http/modules/ngx_http_not_modified_filter_module.c#L78)，Mozilla的说明在：[MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-Modified-Since)

  - 如果存在IMS请求头，并且IMS的值小于资源的最后修改时间，那么返回200，以及新的ETag和Last-Modified
  - 如果存在INM请求头，并且资源的ETag值与INM的值不匹配，那么返回200，以及新的Etag和Last-Modified
  - 否则，认为资源未被修改，返回304，以及空的响应体

  当开启

  ```
  proxy_cache_revalidate
  ```

  时，Nginx会通过If-Modified-Since和If-None-Match请求头，来使过期的缓存条目重新生效

###### 5，$upstream_connect_time

建立到upstream server的连接所花费的时间。单位是秒，精确到毫秒。对于SSL连接，该时间也包含SSL握手所花费的时间。建立多个连接所花费的时间之间，像$upstream_addr中的地址一样，用","和":"分隔

###### 6，$upstream_cookie_*name*

upstream server返回的"Set-Cookie"响应头中的，名称*name*的cookie的值。只有最后一个server返回的响应中的cookie会被保存

###### 7，$upstream_header_time

从upstream server接收响应头所花费的时间。单位是秒，精确到毫秒。多个响应所花费的时间之间，像$upstream_addr中的地址一样，用","和":"分隔

###### 8，$upstream_http_*name*

用于获取upstream server返回的任意响应头的值。
响应头名称和*name*之间的转换方式是：将响应头名称转换成小写形式，并将中划线替换成下划线。
只有最后一个server的响应头会被保存

###### 9，$upstream_response_length

保存从upstream server获取到的响应的长度。单位是字节。多个响应的长度之间，像$upstream_addr中的地址一样，用","和":"分隔

###### 10，$upstream_response_time

保存从upstream server接收响应所花费的时间。单位是秒，精确到毫秒。多个响应所花费的时间之间，像$upstream_addr中的地址一样，用","和":"分隔

###### 11，$upstream_status

保存从upstream server获取到的响应的状态码。多个响应的状态码之间，像$upstream_addr中的地址一样，用","和":"分隔