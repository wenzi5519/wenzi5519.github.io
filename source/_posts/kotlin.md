---
title: kotlin基础语法 01
date: 2017-04-19 10:47:46
tags: [Kotlin,Anko,Java,Android]
categories: 笔记
---

> 语法简洁，逻辑看起来也更清楚了，还能省去findviewbyid，目前这些就是我学kotlin的理由
《Kotlin for android Developers》 可以直接去看这个书，我这个笔记，基本都是书上的东西。
<!-- more -->
## 类
1、定义类使用关键字 ``class ``
```
class MainActivity{}
```
2、有一个默认唯一的构造器，如果一个类没有任何内容可以省略大括号
```
class Person(name:String,age:Int)
```
可以在``init``块中写构造函数的函数体
```
class Person(name:String,age:Int){
    init{

    }
}
```
3、继承
父类需要使用关键字 open 声明
```
open class Anmina(name:String)

class Person(age:Int): Anmina(name)
```
但我们只有单个构造函数，需要从父类继承下来的构造器中指定需要的参数，替换``java``中的``super``调用的。
## 定义函数
使用关键字 ``fun ``
1、明确参数类型，返回值类型
```
fun sum(a:Int ,b:Int) :Int{
    return a+b
}
```
2、不指定返回值类型，可以使用表达式计算出来自行推断
```
fun sum (a:Int,b:Int) =a+b
```
3、函数可返回无意义的值，和java中``void``类似
```
//没有返回值的函数，显式指定``Unit``为返回值  
fun showAddResult(a: Int, b: Int): Unit {  
    println(a + b)  
}  
  
//没有返回值的函数，省略``Unit``的写法  
fun showAddResult2(a: Int, b: Int) {  
    println(a + b)  
}  
```
4、``kotlin``中参数先写名称，在写泛型，而且可以给参数指定默认值
```
fun toast(message:String,time:Int = Toast.LENGTH_SHORT){
    Toast.makeText(this,message,time).show()
}
```
在上面的方法中，第二个参数制定了一个默认值，所以在调用该方法的时候我们，可以选择只传一个参数
```
toast("默认参数，吐司测试")
toast("传参，吐司测试",3)
```
## 基本类型和变量
类型：
数字类型不会自动转型。需要使用相应的方法操作
```
val i:Int=5
val d:Double=i.toDouble()
```

运算符：
> shl(bits) – signed shift left (Java's <<)
shr(bits) – signed shift right (Java's >>)
ushr(bits) – unsigned shift right (Java's >>>)
and(bits) – bitwise and
or(bits) – bitwise or
xor(bits) – bitwise xor
inv() – bitwise inversion

字符串：
``String`` 字符串可以像数组一样，通过下标访问到数据

变量：
变量使用关键字 ``var`` 和`` val ``定义
``val``为不可变量，也可以认为是线程安全的，因为无法改变，所以也不需要定义访问控制
一个重要的概念是：尽可能地使用val 

属性：
```
public class Person{
    var name: String=""
}


var person=Person()
person.name="jack"
val name=person.name
```
如果没有指定，属性会默认使用``getter``和``setter``方法。可以自己指定成自定义的代码。
```
public classs Person {
    var name: String = ""
        get() = field.toUpperCase()
        set(value){
            field = "Name: $value"
        }
}
```
## Anko
Anko是JetBrains开发的用来代替xml生成ui布局的库。
Anko使用扩展函数在Android框架中增添了一些新的功能
比如一个Anko扩展函数可以被用来简化获取一个RecyclerView：
```
val forecastList: RecyclerView = find(R.id.forecast_list)
```

**扩展函数**
扩展函数是指在一个类上增加一种新的行为，甚至我们没有这个类代码的访问权限。
Kotlin中扩展函数的一个优势是我们不需要再调用方法的时候吧整个对象当做参数传入。
扩展函数表现的就像是属于这个类的一样，而且我们可以使用``this``关键字和调用所有的public方法
举个栗子:
```
fun Context.toast(message:CharAequence,duration:Int=Toast.LENGTH_SHORT){
    Toast.makeText(this,message,druation).show()
}
```
这个方法可以在Activity内部直接使用，不需要传入context
Anko已经包括了自己的toast扩展函数，跟上面这个很相似。Anko提供了一些针对CharSequence和resource的函数，还有两个不同的toast和longToast方法：
```
toast("Hello world!")
longToast(R.id.hello_world)
```
**api请求**
kotlin提供的扩展函数让请求变得更加简单。
再举个栗子：
先创建一个Request类
```
public class Request(val url: String) {
    public fun run() {
        val forecastJsonStr = URL(url).readText()
        Log.d(javaClass.simpleName, forecastJsonStr)
    }
}
```
请求很简单地接收一个url，然后读取结果并在logcat上打印json。实现非常简单，因为我们使用``readText``，这是Kotlin标准库中的扩展函数。这个方法不推荐结果很大的响应，但是在我们这个例子中已经足够好了。
**异步请求**
anko提供了非常简单的DSL来处理异步任务，可以满足大部分的需求。
提供``async``函数执行异步线程，通过``UIThread``的方式回到主线程。一个栗子
```
doAsync() {
    Request(url).run()
    uiThread { longToast("Request performed") }
}
```
``UIThread``有一个很不错的一点就是可以依赖于调用者。如果它是被一个``Activity``调用的，那么如果``activity.isFinishing()``返回``true``，则``uiThread``不会执行，这样就不会在``Activity``销毁的时候遇到崩溃的情况了。
假如你想使用``Future``来工作，``async``返回一个``Java Future``。而且如果你需要一个返回结果的``Future``，你可以使用``asyncResult``。
