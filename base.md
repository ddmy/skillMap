# JavaScript
## 一、概念篇
### ES6和ES5的区别
> es6和es5 属于两个不同的版本， es6是对es5的一次改进，新增了一些特性，比如
let const 声明变量，箭头函数，模板字符串，解构赋值，import export导入导出,set map数据结构， class类，async await 异步处理函数，Promise异步编程解决方案，Symbol基本数据类型，Proxy代理等等
---
### var let const 的区别
- var 可以重复声明
- let const 受限于块级作用域，必须先声明 ，后使用
-  const 声明为常量，不可以重新赋值
---
### 使用箭头函数应该注意什么
- this指向父级，并非调用者或者window
- 不能够使用`arguments`对象
- 不能够作为构造函数使用
- 不能够作为`Generator`函数使用（yield）
---
### set map 的区别
- set
  - 成员不能重复
  - 只有键值，没有键名
  - 可以遍历
  - 支持 `add delete has` 方法
- map
  - 本质是键值对的集合
  - 可以遍历和各种数据进行转换
---
### 介绍一下class，为什么会有class
> es6的class 是一个语法糖，它的绝大部分的功能es5也可以实现，class是为了让原型对象方法更加清晰，更像面向对象编程的语法
---
### Promise 是同步的还是异步的
> promise 构造方法是同步的，then方法是异步的
---
### loopEvent 
> 因为js是单线程的，在事件循环机制中，事件任务分为宏任务队列(macrotask)和微任务队列(microtask)，简单来讲，首次开始执行先执行宏任务，执行完一个宏任务之后，检查一遍微任务，有微任务就执行一遍，没有的话再去执行宏任务每次执行完宏任务都要去检查。
- 宏任务
  - setTimoeut
  - setInterval
  - setTimemediate
  - requestAnimationFrame
- 微任务
  - process.nextTick
  - MutationObserver
  - Promise.then
  - Promisr.catch
  - Promise.finaly
---
### 为什么要区分宏任务和微任务
> （个人理解）为了插队，微任务在宏任务之后调用，微任务会在下一个ventloop调用之前执行完毕，并且会将微任务执行当中注册的微任务一并执行完毕，才会开始下一次eventloop，所以如果有新的宏任务就需要等待，由此可见，在js底层实现上，微任务是可以插队的。 如果不区分宏任务和微任务的话，微任务就无法再下一次eventloop执行前插队，而这中间可能会获取到一些状态，那么就无法在下一个宏任务中得到同步。 同步对于视图来说至关重要，这就牵扯到了js单线程的特点。
---
### typeof 返回哪些类型
> function number boolean undefined object
---
### 闭包
> 闭包是在函数外部能够读取到函数内部的变量，所以函数函数定义不会被GC收回，如果过度使用闭包，可能会造成内存泄漏问题
---
### 什么是jsonp
> jsonp的目的是为了目标也回调当前页面的方法并传入参数，是为了解决一些情况下的跨域问题，比如页面引入一个 php脚本， php echo 一个 fn(1,2),就会调用当前页面的fn方法，不受同源策略限制。
---
### DomContentLoaded和onload 的区别
> DomContentLoaded 是当前页面DOM结构加载完毕调用的方法，不包括样式表，图片，外部资源等。 onload是在页面所有资源加载渲染完毕时调用
---
### Promise 有几种状态
> pendding fulfilled reject 当进入reject状态时，会进入catch
---
### 什么是 Reflect
> ES6新增API，主要是提供了一系列操作对象的API
--- 
### 什么是Proxy
> 对象代理，在对象外层建立一层拦截，访问处理对象的属性时，需要经过Proxy处理，可以用来定制拦截行为。vue3重写最大的改动就是将 vue2的`Object.definePrototype`底层实现改写为通过`Proxy reflect`实现
### 理解 async/await以及对Generator的优势
> async await 是用来解决异步编程的， async是 Generator函数的语法糖，async 函数返回一个Promise,使用then方法添加回调优势是:
- 内置执行器，async自带执行器，跟普通函数调用一样，而generator函数需要通过 yield 和 next 执行
- 更好的语义化， async 和 await 相对于 * 和 yield 更具语义化
- 更广的适用性，yield命令后只能是Thunk函数或者Promis对象，async函数中await 可以是promise，也可以是原始类型的值
- 返回值是promis，可以直接用then低啊用回调，generator返回的是Iterator对象。
## 二、安全篇
## 三、HTTP
## 四、框架篇
## 五、服务端
## 六、工程化
## 七、工具篇
## 八、HTML和CSS
## 九、coding，算法，源码实现
### 两个变量值交换
```
let a = 1
let b = 2
[a, b] = [b, a]
```
