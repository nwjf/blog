---
title: 你必须要懂的 ES6 语法
date: 2018-03-20 08:36:50
categories: js
tags: [js, ES6]
---

文章正在编写中。
文章正在编写中。
文章正在编写中。
文章正在编写中。
文章正在编写中。


### 简介
ECMAScript 6.0 (简称ES6) ，ES6 的第一个版本，在 2015 年 6 月发布，正式名称就是《ECMAScript 2015 标准》（简称 ES2015）。

### 参考
阮一峰 [《ECMAScript 6 入门》](http://es6.ruanyifeng.com/)

### let/const
ES6 中新增加了 let 和 const 两个命令，let用于定义变量，const 用于定义常量, 与var的不同之处在于let，const都是块级作用域，具体请看代码

##### let or var

```js
// var
if (true) {
  var test = 'hello word';
}
console.log(test); // hello word;
// let
if (true) {
  let test = 'hello word';
}
console.log(test); // ReferenceError: test is not defined
```

##### const

```js
// 情况一
const test = {a: 1};
test.a = 2;
console.log(test); // {a: 2}
// 情况二
const test = 'hello word';
test = 'test'; // TypeError: Assignment to constant variable.
```

小伙伴们看到这里是不是感觉不可思议，const是定义常量的（常量就是定义过后不可改变的量），不要急，看我慢慢道来，
原因是对于对象型的使用是指针式引用，常量只是指向了对象的指针，对象的内容却已然可以修改，注意数组（array）也是对象，看代码：

```js
let arr = [1, 2, 3, 4];
console.log(typeof arr); // object
```
那问题来了，怎么判断数组呢，解决这个问题前先了解下js的类型，
js中有六种数据类型，包括五种基本数据类型（Number, String, Boolean, Undefined, Null）,和一种复杂数据类型（Object）,如果对js类型比较模糊，请先学习js基础知识。
回到正题，数组类型判断.

```js
let arr = [1, 2, 3];
console.log(arr instanceof Array); // true
console.log(Array.isArray(arr)); // true
```

### 字符串

### 函数
##### 箭头函数
###### 基本用法

```js
var test = v => v;
// 等同于
var test = function (v) {
  return v;
};
// 
var test = () => 5;
// 等同于
var test = function () { return 5 };
//
var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
// 如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
var sum = (num1, num2) => { return num1 + num2; }
```
**注意点**
（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

### class

#### 基本用法
ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。

基本上，ES6 的class可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

```js
// 基本使用
class Point {
  // 类方法
  toString () {}
}
typeof Point; // function
```
类的数据类型就是函数，类本身就指向构造函数。
使用的时候，也是直接对类使用new命令，跟构造函数的用法完全一致。
```js
// 类的两种方法
class Foo {
   // ...
}
const f = new Point(); // Uncaught ReferenceError: Foo is not defined(...)
var Point = class {
  // ...
}
const f = new Point(); // 正确
```
类声明和函数声明不同的一点是，函数声明存在变量提升现象，而类声明不会。即，类必须先声明，然后才能使用，否则会抛出ReferenceError异常。

#### 严格模式
类和模块的内部，默认就是严格模式，所以不需要使用use strict指定运行模式。只要你的代码写在类或模块之中，就只有严格模式可用。

#### constructor
constructor方法是一个特殊的类方法，它既不是静态方法也不是实例方法，它仅在实例化的时候被调用。一个类只能拥有一个名为constructor的方法，否则会抛出SyntaxError异常.

如果没有定义constructor方法，这个方法会被默认添加，即，不管有没有显示定义，任何一个类都有constructor方法。

子类必须在constructor方法中调用super方法，否则新建实例时会报错。因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工，如果不调用super方法，子类就得不到this对象。

#### 静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。
如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
// 静态方法
class Foo {
  static classMethods () {
    console.log('我是静态方法');
  }
  point () {
    console.log('我不是静态方法');
  }
}
Foo.classMethods(); // 我是静态方法
let f = new Foo();
f.classMethods(); // TypeError: foo.classMethod is not a function
f.point(); // 我不是静态方法
```
**注意**
1.如果静态方法包含this关键字，这个this指的是类，而不是实例。
2.静态方法可以与非静态方法重名。
3.父类的静态方法可以被子类继承。

#### 继承

Class 可以通过extends关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。

```js
class Point {}
class SubPoint extends Point {}
```