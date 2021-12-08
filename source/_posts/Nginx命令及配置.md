---
title: Nginx命令及配置
archives: 2021
date: 2021-12-08 08:57:20
categories:
---

## 基本命令
nginx

启动后
nginx -s signal

signal

stop — fast shutdown
quit — graceful shutdown
reload — reloading the configuration file
reopen — reopening the log files


## 静态资源服务器配置

``` 
location / {
    root /data/www;
}

location /images/ {
    root /data;
}
```

root 指定本机资源位置
location 指定资源访问url；优先匹配长路径，后匹配短路径
比如： 访问 http://localhost/  则会访问/data/www
访问 http://localhost/images  则会访问/data/images

## 反向代理
```
server {
    listen 80;
    root /data/up1;
    location / {
        proxy_pass http://localhost:8080;
    }

     location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

当location中和server中同时具有root时，优先选用location中的，没有则使用server中的；

proxy_pass 可以将访问代理到本机的启动的某个服务中，比如这里的8080端口