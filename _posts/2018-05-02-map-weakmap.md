---
layout: post
title: "Map和WeakMap"
date: 2018-05-02
excerpt: "Map和Object的区别是什么呢，WeakMap又是做什么的呢"
tags: [学习]
comments: false
---



# Map和WeakMap

ES6中加入了新的数据结构，Set，Map同时也有对应的WeakSet和WeakMap。之前学习JAVA的时候对这两种数据结构已经很熟悉了，这也是我觉得js和JAVA越来越像（尤其是用了ts以后）的原因。很多时候我经常会记混两者的API，不过我也很久不用JAVA啦。

今天想来讨论一下Map和WeakMap。

在ES6之前，Object被拿来做map使用，毕竟也是储存键值对，同时也是唯一键值。同时也可以做iteration遍历。那么为什么还需要Map呢，Object和Map的区别是什么呢？

## Map和Object的主要区别

1. 最主要的，也是最大的局限性是，Object只接受String或者Symbol作为它的键。当我们使用对象的时候对象会被stringify为'[object object]'，所以即使不同的对象也会覆盖前一个对象的值。

    var m = {};
    var x = { id: 1 },
    var y = { id: 2 };
    
    m[x] = "foo";
    m[y] = "bar";
    
    m[x];							// "bar"
    m[y];             // "bar"

2. 第二点就是Map是有序的而Object是无序的，Map会按照键值对添加进去的顺序保存顺序。（有待商榷！）

3. Object原型上可能有一些属性会和你的key起冲突

其他就是Map有一些方便的api啦，毕竟是一种单独的数据结构，那就简单的来看一下属性和方法。

## Map的属性和方法

Map.size获取当前map键值对的长度。

Map.set(), Map.get()存储键值对

Map.delete() 删除某一个键值对，Map.has()是否存在某个键

Map.entries(), Map.keys(), Map.values() 获取所有的键值对，所有的键，所有的值

## WeakMap

WeakMap的键只可以是对象，不可以是基本的数据类型。并且WeakMap保留的是对键对象的弱引用，主要就是可以帮助GC回收没有使用的对象。GC会回收没有被强引用的对象，所以即使WeakMap中保留了对key的弱引用，它还是可以被GC回收的。

和Map不同的是，它不可以获得size也不可以clear，只有4个方法，set, get, has和delete，也不可以遍历，主要是为了防止在操作Map的时候正好遇到GC，结果就会出问题啦。

### Reference

MDN[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

MDN[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)