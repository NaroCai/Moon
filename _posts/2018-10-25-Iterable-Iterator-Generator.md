---
layout: post
title: "Iterable, Iterator, Generator"
date: 2018-10-25
excerpt: "深入的理解一下迭代器和生成器的定义"
tags: [学习]
comments: false
---

## Iterator

iterator迭代器是满足**迭代器协议**的对象。

迭代器协议是：对象**的next方法是一个无参函数，它返回一个对象，该对象拥有done和value两个属性。**

## Iterable

iterable可迭代对象是满足**可迭代协议**的对象。

可迭代协议：**对象的[Symbol.iterator]值是一个无参函数，该函数返回一个迭代器。**

在ES6中，map，set，array，arguments，string都是可迭代对象。可迭代对象可以有以下好处（用法）：

- 使用for..of
- 使用扩展运算符
- 在yield*中使用
- 解构赋值

## Generator

既是迭代器又是可迭代对象

## yield*

生成器委托，可以委托给其他可迭代对象，调用它们的next()， 得到value。

并且yield*不是一个statement，而是一个expression。它有自己的值。

    function* g4() {
      yield* [1, 2, 3];
      return "foo";
    }
    
    var result;
    
    function* g5() {
      result = yield* g4();
    }
    
    var iterator = g5();
    
    console.log(iterator.next()); // { value: 1, done: false }
    console.log(iterator.next()); // { value: 2, done: false }
    console.log(iterator.next()); // { value: 3, done: false }
    console.log(iterator.next()); // { value: undefined, done: true }, 
                                  // 此时 g4() 返回了 { value: "foo", done: true }
    
    console.log(result);
    
    for (var a of iterator) {
    	console.log(a);// 1, 2, 3 分别输出
    	console.log(result);// undefined (还未执行完，没有到返回值的地方）
    }
    console.log(result); // foo

## Reference:

[MDN 生成器对象到底是一个迭代器还是一个可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#%E7%94%9F%E6%88%90%E5%99%A8%E5%AF%B9%E8%B1%A1%E5%88%B0%E5%BA%95%E6%98%AF%E4%B8%80%E4%B8%AA%E8%BF%AD%E4%BB%A3%E5%99%A8%E8%BF%98%E6%98%AF%E4%B8%80%E4%B8%AA%E5%8F%AF%E8%BF%AD%E4%BB%A3%E5%AF%B9%E8%B1%A1)

[MDN Operators yield*](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/yield*)