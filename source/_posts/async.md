---
title: 'js异步 -- Promise 与 Async / Await'
date: 2018-08-14 11:10:13
categories: js
tags: [js, ES6, ES7]
---

#### Promise
简介:
Promise 是异步编程的一种解决方案
ES6 规定，Promise对象是一个构造函数，用来生成Promise实例。

Promise 一般用于网络和 I/O 操作，比如读取文件，或者创建 HTTP 请求。我们可以创建异步 promise，然后用 then 添加一个回调函数，当 promise 结束后会触发这个回调函数，而非阻塞住当前“线程”。回调函数本身也可以返回一个 promise 对象，所以我们能够链式调用 promise。

promise提供的方法属性


```js
Promise.prototype.then();
Promise.prototype.catch();
Promise.prototype.finally();
Promise.all();
Promise.race();
Promise.resolve();
Promise.reject();
Promise.reject();
```

##### then

then方法的第一个参数是resolved状态的回调函数，第二个参数（可选）是rejected状态的回调函数。

then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。

##### catch

catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。

##### finally

finally方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。
不管promise最后的状态，在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数。
finally方法的回调函数不接受任何参数

##### all

Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
all方法接受一个数组作为参数

```js
const p = Promise.all([p1, p2, p3]);
p
.then()
.catch();
```

p是包含 3 个 Promise 实例的数组，只有这 3 个实例的状态都变成fulfilled，或者其中有一个变为rejected，才会调用Promise.all方法后面的回调函数。


##### race
Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.race([p1, p2, p3]);
```
上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。




##### 封装ajax

```js
const getJSON = function(url) {
    const promise = new Promise(function(resolve, reject){
        const handler = function() {
        if (this.readyState !== 4) {
            return;
        }
        if (this.status === 200) {
            resolve(this.response);
        } else {
            reject(new Error(this.statusText));
        }
        };
        const client = new XMLHttpRequest();
        client.open("GET", url);
        client.onreadystatechange = handler;
        client.responseType = "json";
        client.setRequestHeader("Accept", "application/json");
        client.send();

    });

    return promise;
};
```

#### Async

ES2017 标准引入了 async 函数，使得异步操作变得更加方便。

Async 是定义返回 promise 对象函数的快捷方法。

async如下特点

1. 有内置执行器，不用调用next
Generator函数是需要调用next指令来运行异步的语句，async不需要调用next，直接像运行正常的函数那样运行就可以

2. 有更好的语义化
语义化更明确，相比较于Generator的*和yield，async和await更明确。

3. await后面可以跟promise或者任意类型的值
yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）。

4. 返回一个promise对象，可以调用.then
async直接返回一个promise对象，可以用then和cache来处理。

async 错误处理方法

```js
async function myFunction() {
    try {
        await somethingThatReturnsAPromise();
    } catch (err) {
        console.log(err);
    }
}
```