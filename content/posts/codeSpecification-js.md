---
title: JavaScript开发规范
date: 2019-12-10 18:36:50
categories: 规范
tags: [规范, JavaScript]
---

## 前言

本文档的目标是JavaScript代码风格保持一致，容易被理解和被维护。

## 项目规范
- 项目必须有 README.md 描述文件
    - 项目描述
    - 引用技术描述
    - 代码风格描述
    - 目录结构
    - 运行环境
    - 开发命令，运行命令，以及其他命令
    - 复杂流程介绍等

## 代码风格

- 建议JavaScript使用无BOM的UTF-8编码
- 建议在文件结尾处保留一个空格
- 使用4个空格作为一个缩进层级，不允许使用2个空格或者tab字符
- switch下的case和default必须增加一个缩进

```js
// good
switch () {
    case '1':
        break;
    default:
        break;
}
```

## 代码空格

- 二元运算符两侧必须有一个空格，一元运算符与操作对象之间不允许有空格

```js
// good
var a = !arr.length;
a++;
++a;
a = b + c;
// bad
var a=!arr.length;
a=b+c;
```

- 用作代码块起始的左花括号 { 前必须有一个空格
- if / else / else if / for / while / function / switch / do / try / catch / finally 关键字后，必须有一个空格

```js
// good
if () {}
(function () {})();
while () {}
// bad
if(){}
(function(){})()
```

- 在对象创建时，属性中的 ： 之后必须有一个空格， ： 之前不允许有空格

```js
// good
var obj = {
    a: 1,
    b: 1
};
// bad
var obj = {
    a:1, // bad
    b : 1, // bad
    c :1 // bad
};
```

- 在函数声明，具名函数表达式，函数调用中，函数名和 （ 之间不允许有空格

```js
// good
function funName() {}
var funName = function funName2() {}
// bad
function funName () {}
```

- , 和 ; 前不允许有空格，如果不位于行尾， , 和 ; 后必须跟一个空格

```js
// good
callback(a, b);
// bad
callback(a , b) ;
```

- 在函数调用，函数声明，括号表达式，属性访问， if / for / while / switch / catch 等语句中，() [] 内紧贴括号部分不允许有空格

```js
// good
callback(params1, params2, params3);
list(this.data[this.arr[i]]);
if (length > arr.length) {}

// bad
callback( params1, params2, params3 );
list( this.data[ this.arr[ i ] ] );
if ( length > arr.length ) {}
```

- 单行声明的数组与对象，如果包含元素， {}, [] 内紧贴括号部分不允许包含空格
- 声明包含元素的数组与对象，只有当内部元素的形式比较简单时，才允许写在一行，元素复杂的情况，应该换行书写。
- 在函数声明，函数表达式，函数调用，对象创建，数组创建， for语句等场景中，不允许在 , ;前换行。

```js
// good
var arr = [];
var arr = [1, 2, 3];
var obj = {};
var obj = {a: '1', b: 'j'};
var obj = {
    name: 'obj',
    sex: 1,
    age: 1
};
// bad
var arr = [ ];
var arr = [ 1, 2, 3 ];
var obj = { };
var obj = { a: '1', b: '2' };
var obj = {name: 'obj', sex: 1, age: 1};
var obj = {
    name: 'obj'
    , sex: 1
};
```
- import语句中{}内侧紧贴括号部分不允许有空格
```js
// good
import {mapGetters, mapState} from 'vuex';
// bad
import { mapGetters, mapState } from 'vuex';
```

- 行尾不得有空格
- 每个独立语句结束后必须换行
- 每行不得超过120个字符
- 超长的不可分割的代码允许例外，比如复杂的正则表达式，长字符串不例外
- 运算符处换行时，运算符必须在新行的行首

```js
// good
var isTrue = user.isTrue
    && info.isTrue
    && content.isTrue
    || info.length;

var result = num1
    + num2
    + num3
    + num4;

var html = ''
    + '<div>'
    +     '<div>'
    +         '<h1></h1>'
    +     '</div>'
    + '</div>';

var html = [
    '<div>',
        '<h1></h1>',
    '</div>
].join('');
```

- 当函数调用时，如果有一个或以上参数跨越多行，应当每一个参数独立一样。
    - 这通常出现在匿名函数或者对象初始化作为参数时，如setTimeout等

```js
// good
setTimeout(
    function () {
        console.log('setTimeout');
    },
    300
);
// bad
setTimeout(function() {
    console.log('setTimeout');
}, 1000);
```

链式操作
- 链式调用较长时采用缩进进行调整
- 三元运算符由三部分组成，因此其换行应当对于每个部分的长度不同，形成不同的情况

```js
// good
$('#item)
    .find('.select')
    .eq(1)
    .end();
var result = isTrue
    ? resultA
    : resultB;

// 数组和对象初始化调用，严格按照每个对象的{}在独立一行的书写风格
var array = [
    {},
    {}
];
var array = [
    {
        // code
    },
    {
        // code
    }
];
```

## 代码分号

- 不得省略语句结束的分号
- 在if /else / for / do / while语句中，即使只有一行，也不得省略{}
- 函数定义结束不允许添加分号
```js
// good
var name = '';
if (true) {
    name = '';
}
function test() {}
// bad
var name = ''
if (true) name = '';
function test() {};
```

## 命名

- 变量 使用 camel 命名法
- 常量 使用 全部字母大写，单词间下划线分割 命名法
- 函数 使用 Camel 命名法
- 类 使用 Pascal 命名法
- 函数参数 使用 Camel 命名法
- 类方法属性 使用 Camel 命名法
- 类名 使用名词
- 函数名 使用动宾短语
- boolean类型变量使用is或has开头
- 变量，函数在使用前必须先定义
- 每个var，let，const建议只定义一个变量

```js
// good
let userName = '';
let isTrue = true;
const PI = 3.14;
const USER_NAME = '';
function getStyle() {}
class Test {}

// good
var name = ''

// bad
name = '';

```

- 尽量使用 ===，!==，不适用== ，!=，仅当判断null或undefined时允许使用==null

## 循环

- 不要在循环体中包含函数表达式，事先将函数提取到循环体外。
- 对有序集合遍历时，缓存length
    - 虽然浏览器都对数组长度进行了缓存，但对于一些宿主对象和老旧浏览器的数组对象，在每次length访问时会动态计算元素个数，此时缓存length能有效提高程序性能

```js
// good
for (var i = 0, len = elements.length; i < len; i++) {

}
```

## 类型

### 类型检测

- 类型检测优先使用typeof。对象类型检测使用instanceof。 null或undefined使用==null

```js
typeof variable === 'string'; // string
typeof variable === 'number'; // number
typeof variable === 'boolean'; // boolean
typeof variable === 'function'; // Function
typeof variable === 'object'; // Object

variable instanceof RegExp; // RegExp;
variable instanceof Array; // Array

variable === null; // null
variable === 'undefined'; // undefined;
variable == null; // null or undefined;
```

### 类型转换

- 转换为string时，使用+''。

```js
// good
num + '';
```
- 转换为number时，通常使用+。

```js
// good
+str;
// bad
Number(str);
```

- string 转换为 number ，要转换的字符串结尾包含非数字并期望忽略时，使用 parseInt。
- 使用 parseInt 时，必须制定进制

```js
// good
var width = '200px';
parseInt(width, 10);
// bad
parseInt(width);
```

- number 去除小数点， 使用Math.floor / Math.round / Math.ceil，不使用 parseInt

```js
// good
var num = 3.14;
Math.ceil(num);
// bad
parseInt(num, 10);
```

## 闭包

- 建议在适当的时候将闭包内大对象设置为null

    <br>
    在JavaScript中，无需特别的关键词就可以使用闭包，一个函数可以任意访问在其定义的作用域外的变量。需要注意的时，函数的作用域时静态的，即在定义时决定，与调用的时机和方式没有任何关系。

    闭包会阻止一些变量的垃圾回收，对于较老旧的JavaScript引擎，可能导致外部所有变量均无法回收。

    以下内容会影响到闭包内变量的回收：

    - 嵌套的函数中是否有使用的变量
    - 嵌套的函数中是否有 直接调用eval
    - 是否使用with表达式

    Chakra、V8和SpiderMonkey将受以上因素的影响，表现出不尽相同又较相似的回收策略，而JScript.dll和Carakan则完全没有这方面的优化，会完全整个LexicalEnvironment中的所有变量绑定，造成一定的内存消耗。

    如果有非常庞大的对象，切预计会在老旧的引擎中执行，则使用闭包时，注意将闭包不需要的对象设置为空引用。



## 注释

- 单行注释// 后跟一个空格
- 多行注释 /* */ ，尽量避免使用，尽量使用单行注释代替
- 文档注释 /** */形式
- 文档注释前必须空一行

注释类型说明

> 基本类型使用小写（number, string, (boolean），引用类型使用大写(Object, Function, Array, Date, RegExp...)

| 类型           | 语法                  | 解释          |
|----------------|----------------------|---------------|
|String          |{string}              ||
|Number          |{number}              ||
|Boolean         |{boolean}             ||
|Object          |{Object}              ||
|Function        |{Function}            ||
|RegExp          |{RegExp}              ||
|Array           |{Array}               ||
|Date            |{Date}                ||
|单一类型集合     |{Array<string>}     ||
|多类型           |{number/boolean}   |可能时number类型也可能时boolean类型|
|任意类型         |{*}                   |任意类型|


文件注释

```js
// 文件注释
/**
 * @file describe the file
 * @author wjf
 */

// 命名空间
/**
 * @namespace
 */
```

类
```js
/**
 * 描述
 *
 * @class 类名称
 * @extends dev
 */
```

函数注释，方法注释

```js
/**
 * 函数描述
 *
 * @param {string} name 参数1
 * @param {Object} param 参数2
 * @param {string} param.name 参数2描述
 * @return {Object} 返回值描述
 */
```

事件注释

```js
/**
 * 事件描述
 *
 * @event select#change
 * @param {} params
 */
```

模块注释

```js
/**
 * module description
 *
 * @module
 * @exports {number}
 * @exports {string}
 */
```

其他注释标识符
```js
/**
 * @file 文件
 * @namespace 命名空间
 * @module regexp 模块
 * @constructor 构造函数声明注释
 * @param {string} params 参数注释
 * @class 类
 * @extends dev 继承
 * @overview 对当前代码文件的描述
 * @copyright 版权信息
 * @author wjf 代码作者信息
 * @version 1.2.2 版本信息
 * @throws {string} 抛出异常
 * @createDate 2019 创建时间
 * @example http() 示例展示，使用方法
 * @return {string} 返回值
 */
```