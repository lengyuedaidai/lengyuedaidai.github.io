---
layout: post
title: js使用建议
date: 2016-05-11 22:53:00 +0800
description: js使用建议 # Add post description (optional)
img:  # Add image post (optional)
tags: [javascript] # add tag
---
1. javascript是支持中文变量的。
因为ECMA标准规定javascript语音是基于unicode标准开发的，但是在3.0之前，unicode字符集只能出现在注释或者引号内，其他地方只能用ASCⅡ字符集，3.0之后，unicode字符集支持出现在任何地方，便可以用中文进行命名变量。
如：
```
var 变量="变量";
function 我是谁(){
    alert("你是呆瓜")
}
```
但是考虑到开发习惯，命名规则，或者一些其他编码问题，建议不要用中午进行变量命名。
2. 减少全局变量的污染，全局变量有三种定义方式，①在任何块语句外定义变量②window.a="全局变量"③直接使用未经声明的变量，隐式的全局变量，这种是最容易出现的，需要注意，a="全局变量"。尽量在一开始的时候，声明一个全局对象，把自己需要的变量声明到里面，进行调用，如：
```
var my={};
my.name={"first-name":"冷月"，"last-name":"呆呆"}；
```

3. 二进制小数不能很好的处理十进制小数，最典型的例子：0.1+0.2=0.30000000000000004，这种一般可以先转为整数计算，再增加小数点，如(1+2)/10来计算，或者截取固定小数位数来解决。
4. 检测数据类型-typeof，对于typeof只能验证简单的类型，他的返回值只有"number"，"string"，"boolean"，"object"，"function"，"undefined"六种，null，undefined，NaN值都是属于什么类型呢？object，undefined，number。而想判断null值，很简单：
```
if(null===null){return true};
```

5. 其他的一些类型验证

```
var a = "iamstring.";
var b = 222;
var c= [1,2,3];
var d = new Date();
var e = function(){alert(111);};
var f = function(){this.name="22";};
```

判断已知对象类型的方法： instanceof

```
console.log(c instanceof Array) ---------------> true
console.log(d instanceof Date)---------------> true
console.log(f instanceof Function) ------------> true
console.log(f instanceof function) ------------> false
```

注意：instanceof 后面一定要是对象类型，并且大小写不能错，该方法适合一些条件选择或分支。
 
根据对象的constructor判断： constructor

```
console.log(c.constructor === Array) ----------> true
console.log(d.constructor === Date) -----------> true
console.log(e.constructor === Function) -------> true
```

注意： constructor 在类继承时会出错
eg,

```
function A(){};
function B(){};
A.prototype = new B(); //A继承自B
var aObj = new A();
console.log(aobj.constructor === B) -----------> true;
console.log(aobj.constructor === A) -----------> false;
```
而instanceof方法不会出现该问题，对象直接继承和间接继承的都会报true：

```
console.log(aobj instanceof B) ----------------> true;
console.log(aobj instanceof B) ----------------> true;
```

言归正传，解决construtor的问题通常是让对象的constructor手动指向自己：

```
aobj.constructor = A; //将自己的类赋值给对象的constructor属性
alert(aobj.constructor === A) -----------> true;
alert(aobj.constructor === B) -----------> false; //基类不会报true了;
```

 
通用但很繁琐的方法： prototype

```
alert(Object.prototype.toString.call(a) === ‘[object String]’) -------> true;
alert(Object.prototype.toString.call(b) === ‘[object Number]’) -------> true;
alert(Object.prototype.toString.call(c) === ‘[object Array]’) -------> true;
alert(Object.prototype.toString.call(d) === ‘[object Date]’) -------> true;
alert(Object.prototype.toString.call(e) === ‘[object Function]’) -------> true;
alert(Object.prototype.toString.call(f) === ‘[object Function]’) -------> true;
```

大小写不能写错，比较麻烦，但胜在通用。

6. 小心使用parseInt函数
```
console.log(parseInt("123abc"));-------> 123
console.log(parseInt("1.23"));-------> 1
console.log(parseInt(".123"));-------> NaN
console.log(parseInt("010"));-------> 10
console.log(parseInt("0x10"));-------> 16
console.log(parseInt("123abc",16));-------> 1194684
```

7. 小心js自动断句。

```
var a = function(){
    return
        {
        a:"aa"
        };
}
console.log(a());-------> undefined
 var  i = a?1:b?2:c?3:4
```

8. 一些特殊值的使用：NaN==NaN,isNaN(NaN);Infinity代表无限大，-Infinity表示负无限大；isFinite函数可以判断是否是数字，但是他会自动把字符串先转为数字进行判断，如：isFinite("1")为true；想判断一个值是否是完全是数字，可以通过typeof 和isFinite配合使用；如
```
var isNunber=function=(value){return typeof value ==="number" && isFinite(value)}
```
