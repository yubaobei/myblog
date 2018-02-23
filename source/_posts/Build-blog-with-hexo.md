---
title: "使用hexo快速搭建个人博客"
date: 2018-02-22 10:45:11
tags:
---

# 前言
哈哈哈，很早就想搭建一个属于自己的博客了。（ps:平时做点小笔记也挺方便的~）

# 准备环境
## Nodejs安装
hexo是基于Nodejs的静态博客框架。所以首先需要安装Nodejs。请到官网下载并安装([Nodejs官网下载地址](https://nodejs.org/en/))
安装成功后打开命令行，输入node -v 查看nodejs的版本号

```
node -v

```
![](/images/01.png)

## Git安装

Git 各平台安装包下载地址为：[http://git-scm.com/downloads](http://git-scm.com/downloads)

# 安装Hexo并建立博客网站

``` javascript
// 用npm安装Hexo
npm install -g hexo-cli

// 创建一个文件夹用来存放博客所有的内容
mkdir myblog

// 在指定文件夹下添加建站需要的文件
hexo init myblog
cd myblog/

// 安装package.json里面的依赖包
npm install
ls
_config.yml node_modules/ package.json scaffolds/ source/ themes/

```
_config.yml: 博客的主要配置文件
node_modules： 该文件夹下存放项目运行所有的依赖包
package.json： 应用程序的信息。在运行hexo init后生成
scaffolds： 该文件夹下存放博客的模版，可以在运行hexo new 命令时指定模版
source：资源存放文件夹
themes：主题存放文件夹，hexo支持第三方主题插件，如Next等

此时运行 hexo server 就可以让你的博客服务器跑起来啦，当然是本地的哦~

``` bash
hexo server
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
打开 http://loaclhost:4000/ 就进入博客的主页啦，不过都是一些默认的配置。
下面继续介绍如何将网站托管到github上面，这样就可以随时随地访问自己的博客啦。

# 配置博客

## 建立github仓库
Github的官网： [https://github.com](https://github.com),如果没有帐号的话先建个帐号。
然后建立一个仓库，仓库名的格式必须为： yourGithubName.github.io, 其中yourGithubName是你的github的名字。

![](/images/02.png)

配置git:

``` bash
git config --global user.name "your username"
git config --global user.email "your email"
```

生成ssh并添加到github:

``` bash
ssh-keygen -t rsa -C "youremail@example.com"
```
复制id_rsa.pub文件里面的内容，请按下图步骤添加ssh到github

![](/images/03.png)

![](/images/04.png)

![](/images/05.png)

## 修改配置文件
打开_config.yml文件，修改如下内容：

```
deploy:
  type: git
  repo: git@github.com:yourGithubName/yourGithubName.github.io.git
  branch: master
  message: "{{ now('YYYY-MM-DD HH:mm:ss') }}"

```
仓库的地址repo属性最好配置成ssh的格式，若使用https格式在提交部署时总会提示输入用户名及密码。

# 部署博客到Github
部署项目到github上需要安装hexo-deployer-git模块。

``` bash
// 安装hexo-deployer-git
npm install hexo-deployer-git --save
```
部署到github:
``` bash
// 清除缓存(hexo c)
hexo clean

// 生成静态资源(hexo g)
hexo generate

// 部署到github(hexo d)
hexo deploy
```
部署成功后，打开任意浏览器并输入http://yourGithubName.github.io 就可以随时随地访问你的博客啦！
好了，关于hexo搭建博客就介绍这啦！后面会继续介绍主题的配置， 添加技术、评论功能以及绑定域名等等。
