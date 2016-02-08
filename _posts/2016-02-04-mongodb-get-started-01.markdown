---
layout: post
title: "mongodb 入门 -- 基础知识"
date: 2016-02-04 08:00:16 +800
categories: mongodb
---

## 基础知识

### 文档
文档是一个键值对的有序集,一个jvascript对象。

    {“greeting”:"hello world"}

### 集合
集合是一组文档，如果文档相当于关系型数据库中的一行，集合就是关系型数据库中的一张表

### 子集合
组织集合使用 '.' 分隔不同命名空间的子集合。

例如diascubes下的产品是
diascubes.products，用户是 diascubes.users。

如果对产品进行细分，还可以这样：diascubes.products.cubes表示所有的魔方产品的集合。

### 启动Mongodb

先创建一个启动的配置文件:

mongod.conf

写入以下内容：

    port = 8888 
    dbpath = data
    logpath = log/mongod.log
    fork = true

指定启动时使用该文件的配置

    mongod -f conf/mongod.conf
    
上面的配置制定了端口号，数据存放地址，日志文件，fork表示是否开启子进程

### 运行 mongo shell  

    mongo

mongodb会启动一个本地的server，监听8888端口，如果该端口被占用，则启动失败

默认会连接 test 数据库，并将数据库连接复制给变量 db。 这个变量是通过shell 访问 mongodb 的主要入口点

查看 db 当前指向哪个数据库，可以使用 db 命令

    >db
    test
    
使用 use 命令选择数据库

    >use diascubes
    switch to db diascubes
    
查看 db 指向哪个数据库

    >db
    diascubes
    
通过 db.products 来访问产品集合
    


