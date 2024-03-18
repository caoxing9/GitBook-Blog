# centeros部署常用命令

## 前言

```
记录一些部署项目到服务器常用配置
```

## centeros安装mysql

[安装mysql](https://www.jianshu.com/p/79eefc9b21f3)

* 数据库常用操作

```
连接数据库 mysql -u root -p用戶名 -p密碼
使用数据库use desk_show;
显示数据表show tables;
显示表结构describe desk6_0;
mysql其他命令：

显示数据库 show databases;

创建数据库create database name;

选择数据库use databasename;

执行命令source /root/20151010.sql

直接删除数据库，不提醒drop database name

显示表show tables;

显示具体的表结构describe tablename;

select 中加上distinct去除重复字段mysqladmin drop databasename
删除数据库前，有提示。
```

## linux常用命令

```
查看当前系统版本：cat /etc/redhat-release
Linux 删除文件夹和文件的命令
-r 就是向下递归，不管有多少级目录，一并删除
-f 就是直接强行删除，不作任何提示的意思
删除文件夹实例：
rm -rf /var/log/httpd/access
将会删除/var/log/httpd/access目录以及其下所有文件、文件夹
删除文件使用实例：
rm -f /var/log/httpd/access.log
将会强制删除/var/log/httpd/access.log这个文件
systemctl start nginx 启动nginx服务
查看所有进程
ps aux
查看端口进程
lsof -i:port   // port端口
```
