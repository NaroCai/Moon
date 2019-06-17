---
layout: post
title: "JS中Object的keys是无序的吗？"
date: 2018-11-03
excerpt: "一直有着object是无序的概念，object的keys真的是无序的吗？"
tags: [学习]
comments: false
---

# JS中Object的keys是无序的吗？

之前在讨论一个问题的时候，因为我们的库需要适配es5，只能写es5的语法，然后大佬反复强调object的key是无序的，它的顺序是不能保证的。'never rely on property order'

这好像是约定俗成的一样，object的keys是无序的，是non-deterministic的，不可预测的。但是实际上在ES2015以后，Object.keys的说明变更了，在一些现代的浏览器中，是可以预测的。(ie白白)

## 1如果object的key中都为**自然数**的话，它会按照数字的大小进行排序，如果是非自然数的话就会归类到下一种string类型中去

    const objWithIndices = {
      23: 23,
      '1': 1,
      1000: 1000
    };
    
    console.log(Reflect.ownKeys(objWithIndices)); // ["1", "23", "1000"]
    console.log(Object.keys(objWithIndices)); // ["1", "23", "1000"]
    console.log(Object.getOwnPropertyNames(objWithIndices)); // ["1", "23", "1000"]

包括在for-in循环的遍历中，keys也是按照这个顺序执行的。

## object的keys为string(非自然数)

它是按照时间顺序做排序的

    const objWithStrings = {
      "002": "002",
      c: 'c',
      b: "b",
      "001": "001",
    }
    
    console.log(Reflect.ownKeys(objWithStrings)); // ["002", "c", "b", "001"]
    console.log(Object.keys(objWithStrings));// ["002", "c", "b", "001"]
    console.log(Object.getOwnPropertyNames(objWithStrings));// ["002", "c", "b", "001"]

## 如果是Symbol的话，也是按照时间排序的。

    const objWithSymbols = {
      [Symbol("first")]: "first",
      [Symbol("second")]: "second",
      [Symbol("last")]: "last",
    }
    
    console.log(Reflect.ownKeys(objWithSymbols));// [Symbol(first), Symbol(second), Symbol(last)]

## 如果是所有的相互结合的话

    const objWithStrings = {
      "002": "002",
      [Symbol("first")]: "first",
      c: "c",
      b: "b",
      "100": "100",
      "001": "001",
      [Symbol("second")]: "second",
    }
    
    console.log(Reflect.ownKeys(objWithStrings));
    // ["100", "002", "c", "b", "001", Symbol(first), Symbol(second)]

结果是先按照自然数升序进行排序，然后按照非数字的string的加入时间排序，然后按照symbol的时间顺序进行排序

### References:

Property order is predictable in JavaScript objects since ES2015[[https://www.stefanjudis.com/today-i-learned/property-order-is-predictable-in-javascript-objects-since-es2015/](https://www.stefanjudis.com/today-i-learned/property-order-is-predictable-in-javascript-objects-since-es2015/)]

The traversal order of object properties in ES6[[http://2ality.com/2015/10/property-traversal-order-es6.html#traversing-the-own-keys-of-an-object](http://2ality.com/2015/10/property-traversal-order-es6.html#traversing-the-own-keys-of-an-object)]