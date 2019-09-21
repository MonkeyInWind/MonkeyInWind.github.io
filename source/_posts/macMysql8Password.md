---
title: mac mysql8 密码
date: 2019-09-21 12:47:53
tags:
---
新电脑安装mysql之后第一次是无法登陆的因为没有初始密码，网上都是老版本的处理方法，mysql8已经失效。
###1、停止mysql服务
系统设置偏好 > mysql > Stop Mysql Server
![image.png](1.png)
###2、跳过登陆
```
sudo -i      //root权限
sudo mysqld_safe --user=mysql --skip-grant-tables --skip-networking
```
![image.png](2.png)
##这里不要动！！！
打开另一个终端窗口
```
mysql -u root
```
直接回车就可以登陆mysql
###3、修改密码
在这里
```
show databases;
```
可以看见有个mysql数据库
![image.png](3.png)
```
use mysql;
show tables;
```
可以看见有个user的表，感兴趣可以看一下。
接下来
```
flush privileges;
```
![image.png](4.png)
重置密码
```
alter user 'root'@'localhost' identified by 'new password';
```
关闭终端，重新打开
```
mysql -uroot -p[password]
```
即可登陆mysql
###4、开机启动
```
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents/
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```
