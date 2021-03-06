官网

https://jwt.io/



[TOC]



## 解决什么问题

### 用来替代Session/Cookie认证

因为:

> 1.Session是存放在服务器的内存中的，随着用户量的增加，会导致服务器内存吃紧，压力和开销也明显增大。

> 2.多服务器的Session共享问题 解决方案 

> A.用Nginx 做负载均衡的Session保持
>
> B:通过KV数据库来做Session的共享 如Redis Memcache

### 更好的方案   

### Json Web Token 简称jwt

>用户登录成功的时候，服务器会给用户颁发一个令牌（Token)
>
>每个用户都是不一样的，每次用户访问服务器的时候，只需要带上这个令牌，服务器便可知道具体是哪个用户，
>
>对于客户端而言，它只是一串字符串 ,可以存放在内存，缓存文件，数据库中

## JWT的原理

JWT它是由三个部分组成的，分别是头部信息（Header)，内容信息（Payload），签名（Signature)

#### 头部信息

描述关于该 JWT 的最基本的信息，例如其类型以及签名所用的算法

```
{
	"type":"jwt"
	"alg":"HS256"
}
```

#### 内容信息

##### 标准声明

标准中注册的声明（建议但不强制使用）：
 iss：JWT 签发者
 sub：JWT 所面向的用户
 aud：接收 JWT 的一方
 exp：JWT 的过期时间，这个过期时间必须要大于签发时间
 nbf：定义在什么时间之前，该 JWT 都是不可用的
 iat：JWT 的签发时间
 jti：JWT 的唯一身份标识，主要用来作为一次性 token, 从而回避重放攻击。

##### 公共声明

公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息. 

但不建议添加敏感信息，因为该部分在客户端可解密。

##### 私有声明

私有声明是提供者和消费者所共同定义的声明，一般不建议存放敏感信息，因为 base64 是对称解密的，意味着该部分信息可以归类为明文信息。

 

#### 签名

创建签名需要使用 Base64 编码后的 header 和 payload 以及一个秘钥。将 base64 加密后的 header 和 base64 加密后的 payload 使用. 连接组成的字符串，通过 header 中声明的加密方式进行加盐 secret 组合加密，然后就构成了 jwt 的第三部分。
 比如：HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)



### 加密规则

经过Base64编码的头部信息 + 经过Base64编码的内容信息,一起拼成后的字符串，再和秘钥secret由某个算法规则进行加密后得到一个字符串

如图所示:

![image-20200513144545306](http://qa3sq0khl.bkt.clouddn.com/image-20200513144545306.png)

### 请求流程图

![image-20200513145837462](http://qa3sq0khl.bkt.clouddn.com/image-20200513145837462.png)



### 关于JWT的优点：

1. 由JSON格式组成，所以具有跨平台性，可以支持各种编程语言。
2. 减少了服务器的压力，相比在内存中开辟Session，存储字符串明显来的省力许多，但是这里需要注意用户的规模，虽然这里我们减少了I/O的开销，但是我们却多了加解密的过程，两者需要权衡。
3. 解决了分布式共享Session的问题，不需要再去考虑文章开头所提到的负载均衡策略所带来的问题。

### 关于JWT的缺点：

没有什么东西是完美无瑕的，JWT虽然解决了一些问题，但是也同时带来了一些新的问题，我列举几个：

1. 无法控制单点登录，假设用户在A设备登录了服务器，服务器正常返回JWT，用户又在B设备登陆了服务器，服务器又返回一个JWT，此时2个JWT都是可用的，服务器没办法踢出其中任意设备的用户。
2. 当用户退出登录或者修改密码的时候，JWT没有办法回收，之前和朋友讨论过也看了一些开源项目，大多数的做法就是直接把JWT丢掉，类似删Cookie形式，但是这里还是存在安全隐患，如果被窃取，依旧会造成一些问题，因为这个JWT还是有效可用的。
    可能有些朋友会说，在载体里面设置JWT的有效时间不就好了，但是这样又会导致新的问题，这个有效时间多久合适？太短，会造成用户频繁的过期登录，太长，当发生以上问题的时候，还是要忍耐有效期内的操作。
3. 关于安全操作，回到文章开头的问题，当JWT被抓包窃取后，是不是就可以模拟用户（有效期内）操作了？比如修改用户的订单信息，频繁的调用验证码接口等，这里涉及到URL的重放等攻击



 

## 使用说明

**PHP Version >= 7.2.1**

代码地址: https://github.com/nowakowskir/php-jwt

支持 Sign加密与Verify校验

支持以下算法 

HS256/HS384/HS512/RS256/RS384/RS512

在php项目中使用:

```
composer require nowakowskir/php-jwt
```



## 扩展阅读

https://segmentfault.com/a/1190000013010835