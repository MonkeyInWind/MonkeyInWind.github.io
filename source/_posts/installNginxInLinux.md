---
title: Linux下安装Nginx
date: 2019-09-21 10:49:23
tags:
---
### 1、安装依赖
```
yum install gcc zlib zlib-devel pcre-devel openssl openssl-devel
```
<!--more-->
### 2、下载安装包
```
cd /usr/local
```
http://nginx.org/download/
去上边地址找到自己想要安装的版本复制连接
```
wget http://nginx.org/download/nginx-1.9.9.tar.gz
```

### 3、安装
解压到当前文件夹
```
tar -xvf nginx-1.9.9.tar.gz
cd nginx-1.9.9
./configure
make
make install
```
安装完成
```
cd ..
```
会发现多了一个nginx的文件夹，nginx就被安装在这个目录下（nginx-1.9.9的文件夹和压缩包可以删了），测试
```
cd nginx/sbin
./nginx -t
```
打印如下信息， 安装完成
```
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
```
### 4、常用命令
安装路径 `/usr/local/nginx` 下

开启：
```
./sbin/nginx
```
关闭：
```
./sbin/nginx -s stop (或者：nginx -s quit)
```
重启：
```
./sbin/nginx -s reload
```
查看进程：
```
ps -ef | grep nginx
```
