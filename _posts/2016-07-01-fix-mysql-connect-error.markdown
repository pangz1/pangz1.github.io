---
layout: post
title: "解决mysql can't connect to local mysql server through socket '/tmp/mysql.sock' (2) 的问题"
date: 2016-02-04 08:00:16 +800
categories: mysql
---

今天使用homebrew 安装mysql，安装之后一直报错

	can't connect to local mysql server through socket '/tmp/mysql.sock' (2)

无法连接，试了n种网上找的方法，都失败了

最后在[这里](http://stackoverflow.com/questions/22436028/cant-connect-to-local-mysql-server-through-socket-tmp-mysql-sock-2)找到了解决方案

	ln -s /Applications/MAMP/tmp/mysql/mysql.sock /tmp/mysql.sock

顺便了解了下 ln 这个命令

ln是linux中又一个非常重要命令，它的功能是为某一个文件在另外一个位置建立一个同不的链接，这个命令最常用的参数是 -s , 具体用法是: ln -s 源文件 目标文件