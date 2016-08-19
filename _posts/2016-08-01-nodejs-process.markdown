---
layout: post
title: "nodejs 中的 process 对象"
date: 2016-08-01 12:06:16 +800
categories: nodejs
---

nodejs 提供了为数不多的几个全局变量, 其中最为重要的就是 `process` 对象了, 下面一起来了解一下 `process` 对象

## process


首先看一下它的一些属性和方法

### process.version

node 版本字符串, 例如 'v0.1.103'.

### process.execPath

执行 node 的目录

### process.pid

process id

### process.cwd()

返回当前的工作目录, 例如：

    $ cd ~
    $ node
    $ process.cwd()
      "/Users/pangzi"

### process.chdir(path)

改变当前的工作目录到指定的目录

    $ process.chdir('/foo');

### process.env

包含用户环境变量的对象, 例如

    $ process.env
    { PATH: '/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin',
    TERM: 'xterm-256color',
    HOME: '/Users/pangzi',
    ...
    _: '/usr/local/bin/node' }

### process.argv

使用`node`执行一个文件, 跟在文件后面所有的值都作为参数

    $ node foo.js foo bar baz

在foo.js中我们使用 `console.dir(process.argv)` 打印结果如下:

    [ '/usr/local/Cellar/node5/5.12.0/bin/node',
    '/Users/pangzi/app/pangz1.github.io/foo.js',
    'foo',
    'bar',
    'baz' ]

### process.exit()

### process.on()

process 本身是一个 `EventEmitter`, 我们可以通过 监听`uncaughtException`事件来捕获异常

    process.on('uncaughtException', function (err) {
        console.log('got an error: %s', err.message);
        process.exit(1);
    })

    setTimeout(function () {
        throw new Error('fail')
    }, 100)

### process.kill()

`process.kill()`方法发送一个信号至给定的进程(pid), 默认是发送 SIGINT 信号

    process.on('SIGTERM', function () {
        console.log('terminating');
        process.exit(1)
    })

    setTimeout(function(){
        console.log('sending SIGTERM to process %d', process.pid);
        process.kill(process.pid, 'SIGTERM');
    }, 500);

    setTimeout(function(){
        console.log('never called');
    }, 1000);
