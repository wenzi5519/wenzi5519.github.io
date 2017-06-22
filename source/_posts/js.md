---
title: js基础语法
date: 2017-03-31 10:29:12
tags: [JavaScript]
categories: 笔记
---

### 语法
JavaScript要求每个语句以 ; 结束，语句块使用 {..}。如果没有在语句后面加 ; 并不会报错，浏览器会自动补全，自动加分号，有可能会改变程序语义，所以还是要写。

<!-- more -->
##### 赋值语句
```
var x = 1;
```
##### 字符串
```
'Hello JavaScript';
```
##### alert提示
```
alert('Hello JavaScript');
```
##### 判断
```
if ( 2 > 1) {
    alert('2 > 1');
}
```

> JavaScript中吧 null、undefind、0、NaN、''视为false，其他值都视为true。

##### 循环
1、for循环
```
for (i=0;i<10;i++){
    ...
}
```
for ... in 可以把一个对象的所有属性一次循环出来（对于数组的循环结果是string）
```
for (var key in person){
    alert(key); //name,age,middle-school
}
```
要过滤掉对象继承的属性，用 hasOwnProperty() 来实现:
```
for (var key in person){
    if (o.hasOwnProperty(key)) {
        alert(key); //name,age,middle-school
    }
}
```
2、while循环
```
var x = 0;
var n = 99;
while (n > 0) {
    x = x + n;
    n = n - 2;
}
x; // 2500
```
do...while,与while的区别就是在于，不是每次开始的时候判断条件，而是每次结束的时候判断条件。
```
var n = 0;
do{
    n = n + 1;
}while(n < 100);
n;
```
### 数据类型
##### 整数
JavaScript不区分整数和浮点数，统一使用Number表示

##### 字符串
可以使用 "" 或 '' 表示
多行字符串可以使用\n来写，ES6标准新增了一种多行字符串的表示方法用反引号 `...` 表示：
```
`测试
多行
字符串`;
```
模板字符串：
```
var name = '小明';
var msg = '你好,'+ name;

var msg2 = `你好${name}`;
```
获取字符串某个指定位置的字符，可以使用下标操作
```
var s = 'Hello';
s[0]; //H
```
字符串的一些常用方法:
toUpperCase() 转化为大写
toLowerCase() 转化为小写
indexOf       搜索指定字符串出现的下标
```
var s = 'Hello,world';
s.indexOf('world'); //7
s.indexOf('World'); //没有返回-1
```
substring()   截取字符串
```
var s = 'Hello,world';
s.substring(0,5); //hello
s.substring(7); //world
```
##### 布尔值 
可以使用 true 或 false 表示，也可以是布尔运算的结果

运算符： 相等运算符有两种：
== : 它会自动转化数据类型在比较
=== : 它不会自动转化数据类型，如果类型不一致，返回false，如果一致在比较

**注意：**1.NaN 这个特殊的Number与其他值都不想等，包括自己可以使用 isNaN(NaN) //true 方法判断
2.浮点数比较的时候，浮点数计算过程会产生误差，所以比较的时候要注意 
1/3 === (1-2/3); //false
Math.abs(1/3-(1-2/3))<0.00000001; //true

##### 空值 
在JavaScript中 null 表示空，与 0 、 '' 不同。
undefined则和java中的null类似,表示未定义
大多数情况下，我们都应该用null，undefined仅仅在判断函数是否传递的情况下使用

##### 数组 
数组使用 [] 表示，也可以通过函数来创建,索引从0开始
```
[1,2,3,45,'hello',null,true]
new Array(1,2,3);
```
常用方法：
slice  截取数组，返回一个新的数组
push   在数组末尾追加若干元素
pop    删除最后一个元素
unshift 在数组头部添加若干元素
shift   删除第一个元素
sort    排序
reverse 翻转数组
splice  修改数组
```
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```
concat  连接两个数组，返回新数组
join    可以把当前数组使用指定字符串连接起来,返回连接后的字符串
多维数组 
```
var arr = [[1,2],[2,3],[4,5,6,5]];
```
##### 对象 
由键值对组成，用 对象变量.属性值 的方式调用
```
var person={
    name: 'jack',
    age: 20,
    'middle-school': 'No.1 Middle School'   //存在特殊字符要用 '' 包起来  访问方式person['middle-school'];
};
person.sex = '男'; //新增属性 sex
delete person.sex; //删除属性 sex 或者 delete person['sex'];
'name' in person; //判断person是否有'name'属性返回布尔值
```
##### 变量
JavaScript采用动态语言，使用关键字 var 声明变量，为了避免为用 var 声明变量导致的错误，可以使用strict模式，
在JavaScript代码第一行写：
```
'use strict'
```
##### map和set
ES6规范新引入了数据类型Map
```
var m = new Map([['jack',12],['Alic',15],['Bob',13]]);
```
初始化Map需要一个二维数组，或者直接初始化一个空的Map。
```
var m = new Map();
m.set('jack',12); //添加数据到空的map中
m.has('jack'); //判断是否存在key jack
m.get('jack'); //获取key对应的value
m.delete('jack'); //删除key
```
set，也是一组key的集合，但不存储value，由于key不可重复，所以set中没有重复的数据
```
var s = new Set([1, 2, 3, 3, '3']); //数字3 和字符串3可以同时存在因为是不同的元素
s.add(3); //可以使用add方法添加重复数据，但是不会有效果
s; //Set {1, 2, 3, "3"}
```
##### iterable
ES6规范新引入iterable类型，array、map和set都属于这个类型。可以使用for..of遍历，同时for..of也是ES6引入的新语法。
for...in 由于历史遗留问题，实际遍历的是对象的属性名称
for...of 则修复了这些问题，只循环集合本身的元素
```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    alert(x); // '0', '1', '2', 'name'
}

for (var x of a) {
    alert(x); // 'A', 'B', 'C'
}
```
更好的方法是直接使用iterable内置的forEach方法，它接受一个函数，每次迭代会自动调用该函数。
```
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    alert(element);
});
```
如果对某些参数不感兴趣，由于JavaScript的函数调用不要求参数必须一致，因此可以忽略它们。
```
var a = ['A', 'B', 'C'];
a.forEach(function (element) {
    alert(element);
});
```