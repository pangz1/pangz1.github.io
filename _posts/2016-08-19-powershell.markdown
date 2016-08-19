---
layout: post
title: "了解powershell"
date: 2016-08-19 23:06:16 +800
categories: shell
---

随着 powershell 开源，mac上也可以跑了，今天安装了准备体验一把

### 安装

首先在这个[地址](https://github.com/PowerShell/PowerShell/blob/master/docs/installation/linux.md#os-x-1011)
安装mac版本的 powershell

安装之后，打开命令行输入

    powershell

如果安装成的话会有如下显示：

    PS /User/pangzi>

### 简单的命令-查看进程

输入一个命令显示所有的进程：

    PS /User/pangzi> Get-Process

只显示想要查看的进程：

    PS /User/pangzi> Get-Process -Name qq

<img src="/assets/images/powershell/A2DEE357-705D-40C4-A10B-3F1115879CC2.png">

想要查看更多进程，只需要用逗号","分隔:

    PS /User/pangzi> Get-Process -Name qq, powershell

<img src="/assets/images/powershell/DE0EE0CA-BFF9-4F3F-B1DB-89D980BE1F34.png">

### 简单命令-获取别名

    PS /User/pangzi> Get-Alias

该命令会获取一长串别名列表，比如:

    gal   Get-Alias
    cd    Set-Location   设置工作目录为指定路径
    dir   Get-ChildItem  列出指定目录下的items
    ni    New-Item       创建item(文件)
    sc    Set-Content    设置内容
    type  Get-Content    获取内容

### 简单命令-操作文件
下面具体看看上面几个命令：

列出当前目录及其子目录下的所有文件

    PS /User/pangzi> cd $home
    PS /home> dir -Recurse

列出所有的文本文件

    PS /home> dir -Path *.txt -Recurse

创建一个空的文本文件

    PS /User/pangzi> ni ./test.txt

也可以像下面这样写

    PS /User/pangzi> "" > ./test.txt

可以使用 -Value 来添加内容到文件中，如果文件已经存在，使用 -Force 来覆盖之前的内容

    PS /User/pangzi> ni ./test.txt -Value "Hello powershell!" -Force
    # or
    # #号后面的内容是注释
    PS /User/pangzi> sc -Path ./test.txt -Value "Hello powershell again"

或者

    PS /User/pangzi> "hello powershell" > test.txt

type - Get-Item 获取 item 的内容

    PS /User/pangzi> type -Path ./test.txt

del - Remove-Item

    PS /User/pangzi> del -Path ./test.txt

$PSVersionTable 查看 powershell 的版本

    PS /User/pangzi> $PSVersionTable

exit - 退出

    PS /User/pangzi> exit

Get-Help 获取帮助

    PS /User/pangzi> Get-Help -Name Get-Process
    # demo
    PS /User/pangzi> Get-Help -Name Get-Process -Examples

### 管道

dir 然后 按照大小排序

    PS /User/pangzi> dir | sort-object -property length -descending

dir 获取最大的文件
    # select is short for select-object
    PS /User/pangzi> dir | sort-object -property length -descending | select-object -first 1

### 创建 powershell 脚本

    创建 .ps1 的文件

[本文参考](https://github.com/PowerShell/PowerShell/blob/master/docs/learning-powershell/powershell-beginners-guide.md)


