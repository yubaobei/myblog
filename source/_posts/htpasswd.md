---
title: htpasswd工具
date: 2019-03-19 19:12:26
tags:   
  - htpasswd
  - linux
categories:
  - linux
---

# 介绍
最近公司项目中需要搭建内部docker registry,并且需要给registry添加basic authentication认证。查了一下[docker官方文档](https://docs.docker.com/registry/configuration/#token)， 需要给仓库配置一个htpasswd文件，并且加密算法必须是bcrypt（你牛好吧，哥妥协了）。于是去官方文档学习了下，并记录于此。htpasswd是有apache提供的用户名/密码存储工具。可以应用于Http中basic authentication认证。详情资料请参考[官网](https://httpd.apache.org/docs/2.4/programs/htpasswd.html)

# 安装

## Redhat/CentOS

```angular2html
yum install httpd-tools -y
```
## Ubuntu


```angular2html
apt-get install apache2-utils
```

## OpenSuse

```bash
zypper install apache2-utils
```
安装完成后， 执行 `htpasswd -h` , 出现如下信息即表示安装成功：

```
Usage:
	htpasswd [-cimBdpsDv] [-C cost] passwordfile username
	htpasswd -b[cmBdpsDv] [-C cost] passwordfile username password

	htpasswd -n[imBdps] [-C cost] username
	htpasswd -nb[mBdps] [-C cost] username password
 -c  Create a new file.
 -n  Don't update file; display results on stdout.
 -b  Use the password from the command line rather than prompting for it.
 -i  Read password from stdin without verification (for script usage).
 -m  Force MD5 encryption of the password (default).
 -B  Force bcrypt encryption of the password (very secure).
 -C  Set the computing time used for the bcrypt algorithm
     (higher is more secure but slower, default: 5, valid: 4 to 31).
 -d  Force CRYPT encryption of the password (8 chars max, insecure).
 -s  Force SHA encryption of the password (insecure).
 -p  Do not encrypt the password (plaintext, insecure).
 -D  Delete the specified user.
 -v  Verify password for the specified user.
On other systems than Windows and NetWare the '-p' flag will probably not work.
The SHA algorithm does not use a salt and is less secure than the MD5 algorithm.
```
# 参数说明

-c: 创建一个htpasswd文件
-n: 仅将结果输出到控制台，并不更新文件
-m: 默认选项，采用MD5算法加密密码
-s: 采用SHA算法加密
-d: 采用CRYPT算法加密
-B: 采用bcrypt算法加密（较安全）
-C: 采用bcrypt算法时，使用该选项设置计算时间参数（默认是5，参数范围4-31，时间越长，安全系数越高）
-b: 直接从命令行处接收密码而不用根据提示输入密码
-i: 从标准输入读取密码(用于shell脚本)
-p: 不加密密码，明文保存
-D: 删除指定用户
-v: 验证用户，根据提示输入密码并校验

# 用例

## 创建用户名密码
```angular2html
# 使用默认算法（MD5）加密
htpasswd -cb /tmp/mypasswd admin Admin@111
Adding password for user admin

cat mypasswd 
admin:$apr1$3NDVvhgM$bZMav87W9xWahv5vb7oIt0

# 使用bcrypt加密
htpasswd -cbB /tmp/mypasswd1 admin Admin@111

cat mypasswd1
admin:$2y$05$QdU2Jw1gOwYR1f/MYBjf2.q4bP6nos1sltfpts8LndilJc2y.aY7i
```

## 新增用户名密码
在原来的htpasswd文件里添加新的用户

```angular2html
# 在刚才创建的mypasswd文件中新增一个super-admin用户
htpasswd -b /tmp/mypasswd super-admin Admin@222

cat mypasswd
admin:$apr1$3NDVvhgM$bZMav87W9xWahv5vb7oIt0
super-admin:$apr1$whQCl5w.$QSLkUOX/izGCj/T4S5UFZ1
```

## 删除用户

```angular2html
# 删除mypasswd中admin用户
htpasswd -D /tmp/mypasswd admin

cat mypasswd
super-admin:$apr1$whQCl5w.$QSLkUOX/izGCj/T4S5UFZ1
```

## 更新用户密码

```angular2html
# 更新super-admin用户的密码为Admin@333
htpasswd -b /tmp/mypasswd super-admin Admin@333

cat mypasswd
super-admin:$apr1$lqDD6sqf$TwLC/JHk8iJ0.q/s8syz4/
```

## 校验用户

```angular2html
# 验证密码 （按提示输入密码Admin@333）
htpasswd -v mypasswd super-admin
Enter password: 
Password for user super-admin correct.
```


