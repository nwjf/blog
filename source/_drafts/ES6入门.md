---
title: 你必须要懂的 ES6 语法
date: 2018-03-20 08:36:50
categories: js
tags: [js, ES6, ES2015]
---

### 简介
ECMAScript 6.0 (简称ES6) ，ES6 的第一个版本，在 2015 年 6 月发布，正式名称就是《ECMAScript 2015 标准》（简称 ES2015）。

### 参考
阮一峰 [《ECMAScript 6 入门》](http://es6.ruanyifeng.com/)

### 站点目录
[1. let/const](#letconst)
[2. 字符串](#string)
[3. 函数](#function)
[4. 类class基本用法](#class)



<a id="letconst"></a>

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


<a id="string"></a>

### 字符串
##### 模板字符串
**( `` )** 反引号来定义模板字符串，
```js
// es5
var msg = 'hello word';

var str1 = '<div>' + msg + '</div>';
var str2 = '<div>' +
          '<span>' +
          '</span>' +
          '</div>';
var str3 = '<div>\
            <span>\
            </span>\
            </div>';
// es6
var str1 = `<div>${msg}</div>`;
var str2 = `<div>
            <span>
            </span>
            </div>
            `;
```
**注意**
1. 如果在模板字符串中需要使用反引号，则前面要用反斜杠转义
2. 如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中。
3. 模板字符串中嵌入变量，需要将变量名写在${}之中。
4. 大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。
5. 如果模板字符串中的变量没有声明，将报错。

<a id="function"></a>

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
注意点
（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

##### 函数参数，参数默认值
ES6 之前，不能直接为函数的参数指定默认值，只能采用变通的方法。
```js
// es6之前
function log (x, y) {
  x = x || 1;
  y = y || 2;
  return x + y;
}
log(); // 3
log(2,3) // 5
// es6
function log (x = 1, y = 2) {
  return x + y;
}
// 或者
let log = (x = 1, y = 2) => x + y;

log(); // 3
log(2,3) // 5
```
注意
1. 使用参数默认值时，函数不能有同名参数。
2. 参数变量是默认声明的，所以不能用let或const再次声明。
3. 指定了默认值以后，函数的length属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值后，length属性将失真。
4. 一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域（context）。等到初始化结束，这个作用域就会消失。这种语法行为，在不设置参数默认值时，是不会出现的。

函数参数
```js
// es6之前
function sum () {
  var s = 0;
  for (var i in arguments) {
    s += arguments[i]
  }
  console.log(s);
}
sum(1,2,3,4,5,6); // 21

// es6
function sum (...num) {
  var s = 0;
  for (var i in num) {
    s += num[i];
  }
  console.log(s);
}
sum(1,2,3,4,5,6); // 21
```
注意
1. arguments对象不是数组，而是一个类似数组的对象。所以为了使用数组的方法，必须使用Array.prototype.slice.call先将其转为数组。
2. rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。
3. 函数的length属性，不包括 rest 参数。
4. 只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。

<a id="class"></a>

### class基本用法

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
  static toString () {
    console.log('我是静态方法');
  }
  point () {
    console.log('我不是静态方法');
  }
  log () {
    console.log('我是输出方法');
  }
}
class Point extends Foo {
  log () {
    console.log('重写父级log方法');
  }
}

let f = new Foo();
let p = new Point();

Foo.toString(); // 我是静态方法
f.toString();   // TypeError: f.toString is not a function
f.point();     // 我不是静态方法
f.log();       // 我是输出方法

Point.toString(); // 我是静态方法
p.toString();  // TypeError: p.log is not a function
p.point();     // 我不是静态方法
p.log();       // 重写父级log方法
```
注意
1. 如果静态方法包含this关键字，这个this指的是类，而不是实例。
2. 静态方法可以与非静态方法重名。
3. 父类的静态方法可以被子类继承。
4. ES6 明确规定，Class 内部只有静态方法，没有静态属性。

#### class的getter和setter方法

与 ES5 一样，在“类”的内部可以使用get和set关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```js
class Point {
  get prop () {
    console.log('我是get方法');
  }
  set prop (value) {
    console.log('我是set方法---值为 : ' + value);
  }
}

let p = new Point();
p.prop;     // 我是get方法
p.prop = 'demo'; // 我是set方法---值为 : demo
```

#### 继承

Class 可以通过extends关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。

```js
class Point {}
class Foo extends Point {}
// Foo就具有Point的所有方法，相当于复制
```

###### super

super可以理解为是指向自己超（父）类对象的一个指针，而这个超类指的是离自己最近的一个父类。