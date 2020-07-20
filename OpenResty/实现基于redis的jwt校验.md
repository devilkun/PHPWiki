
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



# openresty安装jwt模块

 [lua-resty-jwt](https://github.com/SkyLothar/lua-resty-jwt) 



### opm包管理器安装

> opm是openresty package manage的简写



```
//安装依赖组件
yum -y install perl-Digest-MD5
yum install yum-utils
添加仓库
sudo yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo
安装命令行工具 resty
sudo yum install openresty-resty
安装命令行工具 opm
sudo yum install openresty-opm


//安装命令   在/usr/local/openresty目录下执行
opm get SkyLothar/lua-resty-jwt
//安装成功后会在
/usr/local/openresty/site/lualib/resty
/usr/local/openresty/lualib/resty
```

**这段可以跳过**

```

把openresty命令加到系统变量PATH里去
vim /etc/profile
在export PATH=这一行的最后面加上
:/usr/local/openresty/bin
然后执行下面这个命令,
source /etc/profile
```



用github里的jwt.luw文件替换

```
github地址:https://github.com/feng99/lua-jwt-redis.git

/usr/local/openresty/site/lualib/resty/jwt.lua
```



# 代码实现

## phalcon的composer jwt组件[未使用]

> https://packagist.org/packages/dmkit/phalcon-jwt-auth

##  JWT.php



## /data/app_config/application.ini

```

[jwtAuth]

; JWT Secret Key 密钥
secretKey = xxxxxxxxxxx

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

```

lua_package_path "/usr/local/openresty/site/lualib/resty/?.lua;;";
lua_shared_dict jwt_key_dict 10m;
resolver 127.0.0.1;

server {
    listen      80;
    server_name phalcondemo.com;
    set         $root_path '/data/web/github/phalcondemo/public';
    root        $root_path;
    index index.php index.html index.htm;

    try_files $uri $uri/ @rewrite;
    location @rewrite {
        rewrite ^/(.*)$ /index.php?_url=/$1;
    }
    set $redhost "127.0.0.1";
    set $redport 6379;
    location ~ \.php {
	   access_by_lua_file /usr/local/openresty/nginx/conf/lualib/jwt.lua;
        fastcgi_index  /index.php;
        #fastcgi_pass   127.0.0.1:9000;
 	   fastcgi_pass unix:/dev/shm/php-cgi.sock;
	    include fastcgi.conf;
        fastcgi_split_path_info       ^(.+\.php)(/.+)$;
        fastcgi_param PATH_INFO       $fastcgi_path_info;
        fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }    
}

```





## LUA脚本代码

### 把nginx的error_log级别设成debug，错误日志里会有lua的错误信息

```
--接口地址白名单
arr = {}
arr["/index/login"]    = 1;
arr["/index/register"] = 1;
arr["/test/index"] = 1;
local requesturi = ngx.var.request_uri;
local ishit = arr[requesturi];
if ishit == 1 then
  ngx.exit(ngx.OK)
end


--从redis中查询数据的函数
local function redkey(kid, key)
    -- get key from redis
    -- nil  (something went wrong, let the request pass)
    -- null (no such key, reject the request)
    -- key  (the key)

    local redis = require "resty.redis"
    local red = redis:new()
    red:set_timeout(100) -- 100ms

    local ok, err = red:connect(ngx.var.redhost, ngx.var.redport)
    if not ok then
        ngx.log(ngx.ERR, "failed to connect to redis: ", err)
        return nil
    end

    if ngx.var.redauth then
        local ok, err = red:auth(ngx.var.redauth)
        if not ok then
            ngx.log("failed to authenticate: ", err)
            return nil
        end
    end

    if ngx.var.reddb then
        local ok, err = red:select(ngx.var.reddb)
        if not ok then
            ngx.log("failed to select db: ", ngx.var.reddb, " ", err)
            return nil
        end
    end

    --hash hget
    --存储为hash结构 时间计算比较麻烦 且官方不推荐使用自带的时间函数
--[[    local res, err = red:hget(kid, key)
    if not res then
        ngx.log(ngx.ERR, "failed to get kid: ", kid ,", ", err)
        return nil
    end
--]]

    --string get
    local res, err = red:get(kid..key)
    if not res then
        ngx.log(ngx.ERR, "failed to get kid: ", kid ,", ", err)
        return nil
    end

    if res == ngx.null then
        ngx.log(ngx.ERR, "key ", kid, " not found")
        return ngx.null
    end

    local ok, err = red:close()
    if not ok then
        ngx.log(ngx.ERR, "failed to close: ", err)
    end

    return res
end




local jwt = require "resty.jwt";
local cjson = require("cjson");

ngx.log(ngx.INFO,ngx.var.http_Authorization);
--获取http Authorization数据 get/post都需要传递
local auth_header = ngx.var.http_Authorization
if auth_header == nil then
    --ngx.exit(ngx.OK)
    ngx.status = ngx.HTTP_UNAUTHORIZED
    ngx.exit(ngx.HTTP_UNAUTHORIZED)
end

--检查是否传递token
local _, _, token = string.find(auth_header, "Bearer%s+(.+)")
if token == nil then
    ngx.status = ngx.HTTP_UNAUTHORIZED
    ngx.log(ngx.WARN, "Missing token")
    ngx.exit(ngx.HTTP_UNAUTHORIZED)
end

--解析token  
local jwt_obj = jwt:load_jwt('R6v7TUC0Xj0nCPzZDwkrgKAqjxZrWUPR', token)
--ngx.say(cjson.encode(jwt_obj));
if not jwt_obj.valid then
    ngx.status = ngx.HTTP_UNAUTHORIZED
    ngx.say("{error: 'invalid token (101)'}")
    ngx.exit(ngx.HTTP_UNAUTHORIZED)
end

--解析userId数据
local userId = jwt_obj.payload['userId']
--从redis中根据userId查询token
if userId > 0 then
    local private_jwt = redkey('jwt', userId)
    if private_jwt == ngx.null then
        ngx.status = ngx.HTTP_UNAUTHORIZED
        ngx.say("{error: 'session not found'}")
        ngx.exit(ngx.HTTP_UNAUTHORIZED)
    else
	--修改请求头信息 增加自定义字段 向后传递给业务服务
        ngx.req.set_header('Authorization', "Bearer "..private_jwt)
        ngx.req.set_header('userId',userId)
    end
else
    ngx.status = ngx.HTTP_UNAUTHORIZED
    ngx.say("{error: '"..jwt_obj.reason.."'}")
    ngx.exit(ngx.HTTP_UNAUTHORIZED)
end

```




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
>    说明,用户未登录,跳转到登录接口  或者返回提示401.  客户端跳转到登录也可以.
>
> }


![](http://qa3sq0khl.bkt.clouddn.com/535DF905-8E16-431a-A9A6-34B67230D20D.png)

## key的时间过期策略

> app冷启动  访问初始化配置接口的时候   set 过期时间为 604800  
>
> 有效期7天    



# 效果

不传递jwt token

![image-20200526102152286](http://qa3sq0khl.bkt.clouddn.com/image-20200526102152286.png)



## 注意事项

1.lua连接redis只支持IP端口方式    不支持域名   消息来源:mark  