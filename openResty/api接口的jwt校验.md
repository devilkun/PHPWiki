[TOC]



# 参考文章

https://www.jianshu.com/p/66d5163b9e99

https://segmentfault.com/a/1190000015677681

https://segmentfault.com/a/1190000007957976

https://blog.csdn.net/daiyudong2020/article/details/53149242

https://github.com/svyatogor/resty-lua-jwt/blob/master/jwt.lua

https://github.com/svyatogor/resty-lua-jwt



# 作用:

用 openresty 来实现 jwt 协议，做 api 访问的验真，可以降低后台系统的复杂程度，

同时还能提高系统鲁棒性，防止被恶意攻击时系统雪崩。



# 模块

叫 [lua-resty-jwt](https://github.com/SkyLothar/lua-resty-jwt) 的，还有基于它做了二次封装的 [openresty-nginx-jwt](https://github.com/ubergarm/openresty-nginx-jwt)





# 代码实现

## phalcon的composer jwt组件[未使用]

> https://packagist.org/packages/dmkit/phalcon-jwt-auth

##  使用了JWT.php



## /data/app_config/application.ini

```

[jwtAuth]

; JWT Secret Key 密钥
secretKey = R6v7TUC0Xj0nCPzZDwkrgKAqjxZrWUPR
;token有效期 7天
exp = 604800

;忽略的url
; ignoreUri[] = /auth/user:POST,PUT
; ignoreUri[] = /auth/application



```

## 服务器代码

### 生成jwt token

> 只携带了userId参数

```
http://phalcondemo.com/test/jwtencode
//生成jwt token 并保存到redis中,并设置过期时间

//在response对象中设置header属性Authorization: Basic token内容
//其实设置哪个字段都行 自定义一个名字 叫token都可以   和客户端约定好就行

public function jwtEncodeAction()
    {
        try {
            //$payload, $key, $alg = 'HS256', $keyId = null, $head = null
            $secretKey = DiHelper::getConfig()->jwtAuth->secretKey;
            $token = JWT::encode(["userId" => "123458"],$secretKey);
            //假设1个小时过期
            //$redis->setex(token,3600);
            $this->getFlash()->successJson(['token'=>$token]);
        } catch (CustomException $e) {
            throw new JsonFmtException($e->getMessage(), $e->getCode());
        }
    }
    
    //$response->setHeader("token",$data['token']);




//
http://phalcondemo.com/test/jwtDecode



```

### 解密jwt token

```
public function jwtDecodeAction()
    {
        try {
            $secretKey = DiHelper::getConfig()->jwtAuth->secretKey;
            $tokenb = 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOiIxMjM0NTgifQ.-XLFdIggONBJsrUSpu16QLfWw6peaY1H-kwzeMbpKqc';
            $body = JWT::decode($tokenb,$secretKey);
            var_dump($body);
        } catch (CustomException $e) {
            throw new JsonFmtException($e->getMessage(), $e->getCode());
        }
    }
    
    浏览器输出信息为: 
    把userId打印出来
    object(stdClass)#98 (1) { ["userId"]=> string(6) "123458" }
```

## Nginx配置



## LUA脚本代码


### 代码逻辑说明

> 请求到nginx   header头里带着token数据
>
> 然后nginx 查询此token是否在redis中存在   
>
> if(token存在){
>
> ​	检查token有效期   剩余时间不足1天,自动往后加1天. 
>
> ​	如果每天都登录,就永不过期.  此处待讨论
>
> }else{
>
> ​	//token不存在
>
>    说明,用户未登录,跳转到登录接口  或者返回提示403.  客户端跳转到登录也可以.
>
> }

