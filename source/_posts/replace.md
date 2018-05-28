---
title: replace方法深入学习
date: 2018-03-03 22:27:43
categories: js
tags: js
---
前几天朋友问到一个聊天系统，需要代替 [表情] ,趁这次机会仔细研究了replace方法,分享下js的replace用法。
### 初级篇
replace() 方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

![文档](replace/replace_1.png)

```js
let strObj = 'hello word';
let str = strObj.replace('l', 'A');
console.log(strObj);　　// hello word
console.log(str);　　　　// heAlo word
```

代码中只将第一个l代替成A，如果想全部替换：
就要使用正则表达式匹配，不会正则表达式的童鞋请先学习正则：

```js
let strObj = 'hello word';
let str = strObj.replace(/l/g, 'A');
console.log(strObj);　　// hello word
console.log(str);　　　　// heAAo word
```

### 高级篇
第一步先仔细看文档
![文档](replace/replace_2.png)

#### $1,$2...用法
将手机号15516661777替换成为155\*\*\*\*1777；

```js
let phone = '15516661777';
let str = phone.replace(/^(\d{3})\d{4}(\d{4})$/, '$1****$2');
console.log(phone);　　// 15516661777
console.log(str);　　　// 155****1777
```

#### $&用法
```js
let str = 'hello word';
let newStr = str.replace(/he/, '$&---');
console.log(newStr); // he---llo word
// $&表示正则所匹配的字符串
```
#### $`,$'用法
```js
let st = 'hello word';
let a = str.replace(/ll/, '$`---');
let b = str.replace(/ll/, '$\'---'); // 我这里使用的是单引号，需要转义，不懂的请先了解js基础
console.log(a); // hehe---o word
console.log(b); // heo word---o word
// $`表示所匹配字符串左侧字符，$'表示所匹配字符串右侧字符
```
$$是量词，间接的说就是直接转义$输出,就不在过多的介绍

### 第二个参数是回掉函数的时候

上例子理解的快，
问题：需要将字符串‘你好[吃饭][开心][高兴][知识]’中的'[...]'替换为指定内容

```js
let str = '你好[吃饭][开心][高兴][知识]';
let obj = [
  {
    name: '吃饭',
    img: 'https://kefirlily.github.io/'
  },
  {
    name: '开心',
    img: 'https://kefirlily.github.io/'
  },
  {
    name: '高兴',
    img: 'https://kefirlily.github.io/'
  },
  {
    name: '知识',
    img: 'https://kefirlily.github.io/'
  },
]
let newStr = str.replace(/\[(.*?)\]/g, (a, b, c) => {
  // 参数a，正则匹配到的内容
  // 其他参数有点多变
  // 如果正则中没有括号，b将表示开始匹配的位置，如果有括号，为括号匹配到的内容
  // 最后一个参数(不是表示第三个参数)表示str；
  console.log(a);
  // [吃饭]
  // [开心]
  // [高兴]
  // [知识]
  for (let i in obj) {
    if (obj[i].name === b) {
      // 所有的[...]将会被替换成对应的img
      return obj[i].img;
    }
  }
});
console.log(newStr); // 你好ABDC
```

replace方法就写到这里了，欢迎指正，大神勿喷.

