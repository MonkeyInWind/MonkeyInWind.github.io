---
title: 用了react-router刷新404 nginx配置
date: 2019-09-21 08:45:06
tags:
---
react项目用了react-router  
发现只有首页可以访问，在子页面刷新时not found  
<!--more-->
nginx配置如下
```
server {  
  server_name xxx.xxxxxxx.com;  
  location / {  
    proxy_pass http://11.11.11.11:1111/; (node服务端口)  
    root html;  
    index index.html index.htm;  
  }
}
```
这是因为他会根据url去找相应路径下的html  
但是react只有一个index.html入口  
需要改成静态路径并且加一行 `try_files $uri /index.html; `  
无论uri是否变化  
都返回index.html  
```
server {  
  server_name xxx.xxxxxx.com;  
  location / {  
    root /xxx/xxx/xxx/www/build;  
    try_files $uri /index.html;  
  }  
  location ^~ /api/ {  
    proxy_pass http://11.11.11.11:1111/;(服务端接口做代理)  
  }  
}   
```
