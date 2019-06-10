---
title: JS开发规范
date: 2019-06-10 18:36:50
categories: 规范
tags: [规范]
---

## 前言

本文档的目标是是JavaScript代码风格保持一致，容易被理解和被维护。

## 项目规范
- 项目必须由 README.md 描述文件
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
- import语句中{}内侧紧贴括号部分不允许由空格
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
    function () {},
    300
);
```

链式操作
- 链式调用较长时采用缩进进行调整
- 三元运算符由三部分组成，因此其换行应当耿军每个部分的长度不同，形成不同的情况

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
|单一类型集合     |{Array\<string\>}     ||
|多类型           |{number \| boolean}   |可能时number类型也可能时boolean类型|
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