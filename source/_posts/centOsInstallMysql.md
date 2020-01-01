---
title: centos 安装mysql8 以及常用sql语句
date: 2019-09-21 12:44:34
tags:
---
安装环境：centos7  
刚租了台服务器安装mysql的时候发现之前的笔记已经不合适了，更新一下。
<!--more-->
#### mysql安装配置
1、检测是否安装过
```
rpm -qa | grep mysql
```
2、删除当前已安装版本
```
rpm -e --nodeps `rpm -qa | grep mysql`
```
3、在线安装
```
yum -y install mysql-server
```
`这里可能会找不到包，如果没有可用的包，按照如下操作`  
去这里http://repo.mysql.com/  
选择最新版本的`mysql-community`的`rpm包`复制链接地址
```
wget http://repo.mysql.com/mysql80-community-release-el7-1.noarch.rpm
rpm -ivh mysql80-community-release-el7-1.noarch.rpm
yum -y install mysql-server
```
4、开启mysql服务
```
service mysqld start
```
5、mysql添加开机启动
```
chkconfig mysqld on
```
6、初始化配置
```
grep 'temporary password' /var/log/mysqld.log    //查看初始密码
2018-12-04T14:08:38.524688Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: #Fwd;l5cl*r*   //初始密码  这很反人类
//下面初始化
whereis mysql_secure_installation    //找到mysql_secure_installation
mysql_secure_installation: /usr/bin/mysql_secure_installation /usr/share/man/man1/mysql_secure_installation.1.gz
/usr/bin/mysql_secure_installation    //直接运行mysql_secure_installation
Securing the MySQL server deployment.

Enter password for user root:            //输入刚才查看的密码

The existing password for the user account root has expired. Please set a new password.

New password:                              //新密码大小写数字加特殊符号

Re-enter new password:              //重复新密码
The 'validate_password' component is installed on the server.
The subsequent steps will run with the existing configuration
of the component.
Using existing password for root.

Estimated strength of the password: 100
Change the password for root ? ((Press y|Y for Yes, any other key for No) :        //直接跳过  选Y的话是重新设置密码
 ... skipping.
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y  //禁止匿名访问
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y  //不允许root远程访问
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y  //删除测试数据库test
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y  重新加载授权信息
Success.

All done!
```   

#### 常用命令
1、开启/关闭mysql服务
```
service mysqld stop/restart
```
2、访问mysql数据库
```
mysql -uroot -p[password]
```
3、显示数据库列表
```
show databases;
```
4、选择数据库
```
use databases;    #数据库名
```
5、显示表
```
show tables;
```
6、显示表结构
```
describe table;     #表名
```
7、新建/删除数据库
```
create database 库名;
drop database 库名;
```
8、建表
```
##demo##
CREATE TABLE user_info(
    -> id varchar(30) NOT NULL,
    -> user_name varchar(10),
    -> password varchar(10),
    -> PRIMARY KEY ( `id` )
    -> );
```
9、删除表
```
drop table 表名;
```
10、清空表中数据
```
delete from 表名;
```
11、显示表中所有数据
```
select * from 表名;
```
12、表中添加一列  
如果想在一个已经建好的表中添加一列：
```
alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null;
```
这条语句会向已有的表中加入新的一列，这一列在表的最后一列位置。如果我们希望添加在指定的一列：
```
alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null after COLUMN_NAME;
```
注意，上面这个命令的意思是说添加新列到某一列后面。如果想添加到第一列的话：
```
alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null first;
```
13、修改一列数据长度/类型
```
alter table user modify column id varchar(20);
```
14、删除列
```
alter table user drop column id;
```
15、中文显示？？？
```
service mysqld stop      #关闭mysql
whereis my.cnf          #确定配置文件位置
vim /etc/my.cnf          #具体情况看自己的路径
#[mysqld]下加以下两行
character_set_server=utf8
init_connect='SET NAMES utf8'
#保存退出
service mysqld start
```
`需要注意的是，之前在默认情况下创建的表的编码格式并不会改变！所以，如果想让在修改编码格式之前就创建好的表也修改，使用如下指令`
`1.修改数据库的编码格式`
```
alter database <数据库名> character set utf8;
```
`2.修改数据表格编码格式`
```
alter table <表名> character set utf8;
```
`3.修改字段编码格式`
```
alter table <表名> change <字段名> <字段名> <类型> character set utf8;
//demo
alter table user change username username varchar(20) character set utf8 not null;
```
`**修改完的数据库和库里的表 并不会使原来的数据生效，而是新加入的数据才会生效。`
