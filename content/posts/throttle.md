---
title: 优化之路-js函数防抖与函数节流
date: 2018-04-13 20:10:13
categories: JavaScript
tags: [JavaScript, 优化]
---

#### 函数防抖（debounce）

函数防抖是在函数节流的基础上，每隔固定的时间，不管定时器触发没触发，都会执行一遍自定义函数。

单反也有相似的概念，在拍照的时候手如果拿不稳晃的时候拍照一般手机是拍不出好照片的，因此智能手机是在你按一下时连续拍许多张， 能过合成手段，生成一张。翻译成JS就是，事件内的N个动作会变忽略，只有事件后由程序触发的动作只是有效。

函数防抖的应用场景，最常见的就是用户注册时候的手机号码验证和邮箱验证了。只有等用户输入完毕后，前端才需要检查格式是否正确，如果不正确，再弹出提示语。以下还是以页面元素滚动监听的例子，来进行解析：

以邮箱验证为例

```js
var debounce = function () {
    var run = setTiemout(function () {
        clearTimeout(run);
        // 函数体，操作内容
    }, 380);
}
$('.phone').chenge(debounce);
```

优点：
函数防抖的合理应用能够帮助我们充分节省cpu，内存等资源，同时又通过一定的时延间隔去执行自定义函数，在一些频繁的DOM操作，http请求应用中有效提升用户体验。

#### 函数节流（throttle）

当持续触发事件时，保证一定时间段内只调用一次事件处理函数。

节流通俗解释就比如我们水龙头放水，阀门一打开，水哗哗的往下流，秉着勤俭节约的优良传统美德，我们要把水龙头关小点，最好是如我们心意按照一定规律在某个时间间隔内一滴一滴的往下滴。

函数节流会用在比input, keyup更频繁触发的事件中，如resize, touchmove, mousemove, scroll。throttle 会强制函数以固定的速率执行。因此这个方法比较适合应用于动画相关的场景。

咦页面滚动为例
```js
var throttle = function () {
    var time = Date.now();
    return function () {
        var nt = Date.now();
        if (nt - time >= 2000) {
            time = Date.now();
            // 函数体2秒执行一次
        }
    }
}
$(window).scroll(throttle);
```

优点：
使得一定时间内只触发一次函数。原理是通过判断是否到达一定时间来触发函数。



优化之路刚刚开始，欢迎继续查阅。
如有问题请联系newwjf@outlook.com