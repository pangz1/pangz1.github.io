---
layout: post
title: "Generator 函数"
date: 2016-04-10 20:46:16 +800
categories: es6
---
Generator 函数从语法上可以理解成一个状态机，封装了很多内部状态。<br>
执行Generator函数会返回一个遍历器对象，返回的遍历器对象可以依次遍历Generator函数内部的每一个状态

囧...不知道在说些什么

直接看代码吧

    function *helloGenerator(){
      yield 'hello';
      yield 'world';
      return 'ending';
    }
    var hw = helloGenerator();
    hw.next();

上面的函数有三个*状态* : hello, world 和 return 语句
调用 `helloGenerator()` 并不直接执行，也不返回函数运行结果，而是返回一个指向内部状态的*指针对象*, 该对象是一个Iterator(遍历器)对象。
每次调用该Iterator 对象的 next 方法，内部指针就会移到下一个状态，直到遇到下一个 yield。

yield 语句返回一个对象，该对象包含一个 value 属性和一个 done 属性，value 属性就是 yield 后面的值，done 属性表示遍历是否结束，例如上面的 helloGenerator 会一次返回
        
    {value: 'hello', done: false}
    {value: 'world', done: false}
    {value: 'ending', done: true}

遇到 return 语句表示遍历结束，done 属性为true

其实准确的说，yield 语句并没有返回值，yield 输出的值是作为 next 函数的返回值， yield 语句返回的是 undefined



