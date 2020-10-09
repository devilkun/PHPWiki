### 在Http Response Header头信息中,增加自定义字段

> 下面这个样例  自定义了tyname字段
>
> 也可以      add_header  serverId  pro1  
>
> 表示服务器的机器标记

```
server {
    listen 80;
    server_name _;
    access_log /data/logs/access_nginx.log combined;
    root /data/wwwroot/default;
    index index.html index.htm index.php;
    #error_page 404 /404.html;
    #error_page 502 /502.html;
    add_header tyname $host;


    #请求转发
  location /userServer/ {
    proxy_pass http://127.0.0.1:8085/;
  }
```



### 请求转发

```

```



### 反向代理

```

```





### 在Nginx解决跨域问题

```

```





### 请求转发时,携带真实的IP

```

```



