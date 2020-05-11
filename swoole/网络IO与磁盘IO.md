已经知道Swoole是采用了IO复用的方式,提高性能

这里特指的是网络IO

那么在实际的业务中,哪些行为是网络IO 哪些行为是磁盘IO呢?

## 网络IO慢得原因
网络IO主要延时由： 服务器响应延时 + 带宽限制 + 网络延时 + 跳转路由延时 + 本地接收延时 决定。（一般为几十到几千毫秒，受环境干扰极大）
网络IO比磁盘IO更慢

## 网络IO行为

1.获取mysql 连接,并查询数据,传输数据[select/update]

2.获取redis连接,并查询数据,传输数据[get/set]
> 这里要注意  redis比mysql快的原因
> 获取连接都是网路IO,但是具体的数据操作,redis在内存中,mysql在磁盘中,
> 内存IO 比  磁盘IO快几十倍.

3.获取elasticSearch连接

4.请求其他http接口

## 磁盘IO行为
1.mysql引擎在磁盘中查找数据      数据以 B+Tree的方式,存储在磁盘的物理文件中

2.图片上传  导出为Excel  解析Excel  等等对文件的处理行为

3.elasticsearch 查找数据 也是磁盘io  因为数据是以倒排索引的方式存储在磁盘上

![preview](https://pic4.zhimg.com/v2-0bca913bed8f7d40ac523dbb7688da07_r.jpg)






