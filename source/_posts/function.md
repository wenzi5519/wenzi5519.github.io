---
title:  JavaScript函数
date: 2017-04-01 09:49:26
tags: [JavaScript]
categories: 笔记
---
### 定义函数
<!-- more -->
在js中函数的定义方式如下：
```
function a(x){
     if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```
在js中函数也是一个对象，所有可以写成如下形式:
```
var a = function (x){
     if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
```
第二种方式属于匿名函数，但是因为赋值给了变量a，所以两种定义是一样的，但是第二种方式要注意在末尾加 ； 结束赋值语句。
### 调用函数
由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数，同样的少传也没有关系。
```
a(10);
abs(10, 'blablabla'); 
abs(); // 返回NaN
```
为了避免由于收到undefined参数二导致的输出结果为NaN，可以对参数进行判断
```
function a(x) {
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```
### arguments
js中的一个关键字，只能在函数内部起作用，永远指向当前函数的调用者传入的所有参数。
```
function foo(x) {
    alert(x); // 10
    for (var i=0; i<arguments.length; i++) {
        alert(arguments[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);
```
利用arguments可以获得传入的参数，即使没有定义参数，也可以拿到参数值 arguments.length === 0
最常见的用法是判断传入参数的个数
```
// foo(a[, b], c)
// 接收2~3个参数，b是可选参数，如果只传2个参数，b默认为null：
function foo(a, b, c) {
    if (arguments.length === 2) {
        // 实际拿到的参数是a和b，c为undefined
        c = b; // 把b赋给c
        b = null; // b变为默认值
    }
    // ...
}
```
### rest参数

ES6引入了rest可变参数
```
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```
### 变量提升
js的函数有一个特点，会先扫描函数体的语句，把所有声明的变量“提升”到函数的顶部
```
function foo() {
    var x = 'Hello, ' + y;
    alert(x);
    var y = 'Bob';
}

相当于

function foo() {
    var y; // 提升变量y的申明
    var x = 'Hello, ' + y;
    alert(x);
    y = 'Bob';
}
``` 
### 常量
ES6引入了const定义常量
### 方法
在一个对象中绑定函数，称为这个对象的方法。
```
var person={
    name: '小明',
    brith: 1994,
    age： function(){
        var y = new Date().getFullYear();
        return y - this.brith;
    }
}
```
### apply
可以接收两个参数，第一个参数就是需要绑定的this变量，第二个参数是Array，表示函数本身的参数。
```
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```
