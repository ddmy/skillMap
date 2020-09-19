# 前端基础知识点总结
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
---
### target、currentTarget的区别？
> currentTarget当前所绑定事件的元素, target当前被点击的元素
---
### 继承
> 继承首先需要一个父类
```
function People(name){
  //属性
  this.name  = name || Annie
  //实例方法
  this.sleep=function(){
    console.log(this.name + '正在睡觉')
  }
}
//原型方法
People.prototype.eat = function(food){
  console.log(this.name + '正在吃：' + food);
}
```
1. 原型链继承, 父类的实例作为子类的原型
```
function Woman(){ 
}
Woman.prototype= new People();
Woman.prototype.name = 'haixia';
let womanObj = new Woman();
```
- 优点：简单易于实现，父类的新增的实例与属性子类都能访问
- 缺点：
  - 可以在子类中增加实例属性，如果要新增加原型属性和方法需要在new 父类构造函数的后面
  - 无法实现多继承
  - 无法实现多继承，创建子类时，不能向父类构造函数传参数
2. 借用构造函数继承（伪造对象、经典继承）
> 复制父类的实例属性给子类
```
function Woman(name){
 //继承了People
  People.call(this); //People.call(this，'wangxiaoxia'); 
  this.name = name || 'renbo'
}
let womanObj = new Woman();
```
- 优点：
  - 解决了子类构造函数向父类构造函数中传递参数
  - 可是实现多继承
- 缺点：
  - 方法都在构造函数中定义，无法复用
  - 不能继承原型属性/方法，只能继承父类的实例属性和方法
3. 实例继承（原型式继承）
```
function Wonman(name){
  let instance = new People();
  instance.name = name || 'wangxiaoxia';
  return instance;
}
let wonmanObj = new Wonman();
```
- 优点：
  - 不限制调用方式
  - 简单，易实现
- 缺点：
  - 不能多次继承
4. 组合式继承
> 调用父类构造函数，继承父类的属性，通过将父类实例作为子类原型，实现函数复用
```
function People(name,age){
  this.name = name || 'wangxiao'
  this.age = age || 27
}
People.prototype.eat = function(){
  return this.name + this.age + 'eat sleep'
}

function Woman(name,age){
  People.call(this,name,age)
}
Woman.prototype = new People();
Woman.prototype.constructor = Woman;
let wonmanObj = new Woman(ren,27);
wonmanObj.eat(); 
```
- 优点：
  - 函数可以复用
  - 不存在引用属性的问题
  - 可以继承属性和方法，并且可以继承原型的属性和方法
- 缺点：
  - 由于调用了两次父类，所以产生了两份实例
5. 寄生组合继承
> 通过寄生的方式来修复组合式继承的不足，完美的实现继承
```
//父类
function People(name,age){
  this.name = name || 'wangxiao'
  this.age = age || 27
}
//父类方法
People.prototype.eat = function(){
  return this.name + this.age + 'eat sleep'
}
//子类
function Woman(name,age){
  //继承父类属性
  People.call(this,name,age)
}
//继承父类方法
(function(){
  // 创建空类
  let Super = function(){};
  Super.prototype = People.prototype;
  //父类的实例作为子类的原型
  Woman.prototype = new Super();
})();
//修复构造函数指向问题
Woman.prototype.constructor = Woman;
let womanObj = new Woman();
```
6. ES6继承
```
class People{
  constructor(name='wang',age='27'){
    this.name = name;
    this.age = age;
  }
  eat(){
    console.log(`${this.name} ${this.age} eat food`)
  }
}
//继承父类
class Woman extends People{ 
   constructor(name = 'ren',age = '27'){ 
     //继承父类属性
     super(name, age); 
   } 
    eat(){ 
     //继承父类方法
      super.eat() 
    } 
} 
let wonmanObj=new Woman('xiaoxiami'); 
wonmanObj.eat();
```
> ES5继承和ES6继承的区别: es5继承首先是在子类中创建自己的this指向，最后将方法添加到this中`Child.prototype=new Parent() || Parent.apply(this) || Parent.call(this)` es6继承是使用关键字先创建父类的实例对象this，最后在子类class中修改this
---
### requestAnimationFrame
> 相比于`setTimeout setInterval`的优势
> 以帧为单位改变元素轨迹样式的动画
- requestAnimationFrame会把每一帧中所有的元素操作集中起来，在一次重绘或者回流中中执行完毕，重绘回流的时间一般紧紧跟随浏览器的刷新频率，一般是每秒60帧
- 在隐藏不可见的元素中，requestAnimation不会进行重绘和回流，这意味着更少的cpu，gpu，内存的使用
## 二、安全篇
## 三、HTTP
### 页面缓存原理
### 跨域解决方案
### 输入URL到页面展现的过程
> performance.timin，缓存相关，网络相关，浏览器相关 https://www.cnblogs.com/yinhaiying/p/11984003.html
- 输入url
- DNS解析
- TCP握手
- HTTP请求
- HTTP响应数据
- 浏览器解析并渲染页面
### get和post的区别
## 四、框架篇
### vue 响应式原理
> vue实例化的时候将`data`方法返回的数据都挂载上`setter`方法，`setter`方法将页面上的属性进行绑定，在页面`DomContented`事件触发后，vue实例调用`mounted`方法，开始获取接口等异步数据，赋值时，触发提前设置好的`setter`方法，引起页面联动，达到响应式的效果。
---
### diff算法
> 广度优先算法，时间复杂度O(n),比较两颗DOM树差异，是算法最核心的部分，所谓的 Virtual Dom 的diff算法，两棵树的完全的diff算法是一个时间复杂度为O(n^3)的问题，但在前端领域，很少会出现跨越层级的移动DOM元素,所以vue 中只会对同一层级进行对比,这样算法的复杂度就会达到O(n)
---
### 编译原理
- 词法分析（把字符串模板解析成一个个令牌token）
- 语法分析 （生成抽象语法树 AST）
- 解析器 (根据AST生成渲染函数)
---
### 什么是静态标记
> vue template 经过parse过程后生成AST，通过 `isStaic`方法判断 AST是否是静态并打上标记，如果递归父节点素有子节点都是静态节点，那么就会给当前父节点打上静态标记。 和vue3相比，vue3打的静态标记颗粒度更小，可以精确到任意节点，这也是vue3性能高于vue2的原因之一。
---
### hooks以及各个框架的区别
> 生命周期函数
---
### vue3.0 的改动
- 异步组件
  - 异步组件需要使用 `definAsyncComponent`方法来注册
  - component 选项重命名为loader
  - 加载函数本身不接收`resolve`和`reject`传递参数，方法始终返回Promise
- 自定义指令
  - 注册自定义指令的配置参数调整，vue3 的处理方式为在不同生命周期的回调函数处理
- data 更新
  - vue3 data 定义只能是function
- event bus
  - 删除 应用实例的`$on, $off, $once`, eventHub 参数传递方案需要重写， 官方推荐第三方插件实现(mitt)
- 过滤器
  - 删除过滤器（考虑学习成本和实现成本），推荐使用计算属性或者方法调用来实现
- template组件结构
  - 组件支持多个根结点，可以通过`inheritAttrs: false`来禁用组件属性继承
- 函数组件
  - vue3 移除 `functional`
  - `listeners` 作为 一部分传递到 `$attrs`，可以删除
---
### computed的特点？
> 基于依赖的数据会进行缓存结果，减少计算次数。 而watch一般会应用于异步数据变化响应。
---
### vue 如何检测数组变化的
> vue重写了数组的 `push、pop、shift、unshift、splice、sort、reverse`的方法，在数组内容发生改变时，除了调用数组原始方法外，额外调用了`ob.dep.notify()`方法通知订阅者。
---
### 为何vue采用异步渲染
> vue是组件及更新的，如果不采用异步渲染，每次数据更新都会对当前组件进行重新渲染，所以为了性能考虑，在本轮数据更新完毕后，再去更新视图
---
### watch 中 deep:true 如何实现
> watch 初始化时会对需要监听的数据初始化成watcher并收集起来，数据更新时会触发对应的watcher。因为数组对象都是弱引用，所以一般要监听数组对象的改动需要增加`deep: true`的配置。如果存在该配置，vue会对当前对象的每一个key进行求值，然后将当前的watcher存入到对应的属性依赖当中去，这样，对象数组反生变化就会通知数据更新。
---
### vue事件绑定原理
> 通过 `v-on` 或者 `@` 来绑定事件并指定事件执行函数
---
### v-for和v-if 为什么不能一起使用
> v-for 的优先级高于v-if，放在一起时，其实会有一些不需要的数据被渲染，增加性能损耗。可以先将要处理的数据进行过滤，再渲染。
---
### v-model 实现原理，如何自定义v-model
> vue在渲染html模板时，遇到v-model时，会在当前节点绑定一个input事件，监听页面输入的值，从而实现同步更新的目的
---
### 组件中的data为什么是一个函数
> 以函数返回值的形式定义，是为了保证每次复用组件，都能生成一套新的数据，类似于给每个组件创建一个私有数据空间，不会相互影响。
---
### vue 组件通信
- `props $emit`
- eventBus
- vuex
- `$attrs $listener`
- `provied inject`
- `$parent $children ref`
---
### 什么是作用域插槽
> 通过`name`属性分为匿名插槽和具名插槽，作用域插槽可以理解为可以在父组件拿到子组件内的数据，子组件可以在`slot`标签上绑定属性值，父组件可以在`slot-scop`属性绑定的对象上拿到子组件传过来的数据
---
### vue常见的性能优化
1. 绑定的事件和自定义事件及时销毁
2. 合理使用`kee-alive`进行缓存组件，被缓存起来的组件中如果要销毁部分数据（比如，定时器），要放在`deactivated`方法中完成。
3. 使用异步组件
4. 合理使用`v-if` `v-show` `computed` `watch`, `v-for` 要配合`key` 使用，避免和`v-if` 同时使用
5. 组件复用，抽离`mixin`,提取公用函数
6. 改成多页面入口
  - 打包优化
    - 关闭sourceMap生成
    - 使用CDN方式加载外部资源
    - 使用CDN图片（oss图片压缩）
    - 第三方组件按需引入等等
---
### v-for中为什么要使用key
> vue中的diff算法需要通过tag和key来判断是否是sameNode,减少渲染次数，提升渲染性能
---
### vue组件是如何渲染和更新的
> 解析模板为`render`函数，触发vue响应式，监听data属性的`getter` `setter`,执行`render`函数，生成vnode, patch等
---
### vue 相同的逻辑如何抽离
- mixin 混入，抽离公共方法数据
- 组件级复用，通过参数配置动态数据
---
### 为什么要使用异步组件
- 减少应用首次请求资源体积
- 提高资源渲染速度
- 提升页面性能
---
### 对keep-alive的了解
> 缓存数据不需要reset的组件，有两个生命周期函数`activted` `deactivated`
---
### 实现hash路由和history路由
> hash 路由核心是页面的 `hashChange` 事件，监听hash变化。
> history 路由是依赖`history` API `history.pushState` `history.replaceState` 等
---
### vue-router中有哪些导航守卫
- `router.beforeEach` 导航触发调用
- `router.beforeResolve` 导航被确认之前，所有组件内守卫和异步组件被解析之后调用
- `router.afterEach` 导航确认触发之后调用
- `beforeEnter` 路由独享守卫
- `beforeRouteEnter` 组件内守卫
- `beforeRouteUpdate` 组件内守卫
- `beforeRouteLeave` 组件内守卫
> 完整的导航解析流程:
1. 导航被触发。
2. 在失活的组件里调用 beforeRouteLeave 守卫。
3. 调用全局的 beforeEach 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
7. 在被激活的组件里调用 beforeRouteEnter。
8. 调用全局的 beforeResolve 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 afterEach 钩子。
11. 触发 DOM 更新。
12. 调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。
---
### action和mutaion的区别
> Mutaion 是同步函数，Action是异步函数，属于一种约定，为了更好的管理vuex的状态，我们应该遵守
---
### vuex工作原理
> 侵入每一个vue组件并并注册`$store`属性，而所有`$store`都指向同一个tore实例，这样就实现了数据共享
---
### vue ssr生命周期
> 只有 `beforeCreate` 和 `created` 会在服务端执行, 其他任何vue的生命周期方法都不会在服务端执行。
`nuxt` 是 `nuxtServerInit` `route Middleare` `valite` `asyncData` `fetch` `mounted`
---
### 虚拟dom vue和react的区别
> vue 通过数据劫持能比较精确的知道数据变化，不需要特别优化，性能就比较好. react默认是通过比较引用的方式比较的，如果不做优化，可能会导致大量的VDOM重新渲染
---
### 小程序
### swiper
### bootstrap
### elementui
### vant
### axios
## 五、服务端
### 反向代理，负载均衡
### 洋葱模型
### 中间件
### 状态码
### nodejs 中 require是如何工作的？
### koa
### express
### egg
### redis
## 六、工程化
### 了解过的打包工具
### webpack 构建性能优化
### 热更新原理
### vite
### npm优化
### soourceMap的了解
## 七、工具篇
### git
### nginx
### docker
### 浏览器（渲染原理）
### typescript
### 单元测试jest
### 常用的sell命令
- curl
- ping
- telnet
- tail
### ab压测
## 八、HTML和CSS
### 1rem、1em、1vh、1px各自代表的含义？
> rem
rem是全部的长度都相对于根元素<html>元素。通常做法是给html元素设置一个字体大小，然后其他元素的长度单位就为rem。

em
子元素字体大小的em是相对于父元素字体大小
元素的width/height/padding/margin用em的话是相对于该元素的font-size
vw/vh
全称是 Viewport Width 和 Viewport Height，视窗的宽度和高度，相当于 屏幕宽度和高度的 1%，不过，处理宽度的时候%单位更合适，处理高度的 话 vh 单位更好。

px
px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。

一般电脑的分辨率有{1920*1024}等不同的分辨率

1920*1024 前者是屏幕宽度总共有1920个像素,后者则是高度为1024个像素
### calc, support, media各自的含义及用法
> @support主要是用于检测浏览器是否支持CSS的某个属性，其实就是条件判断，如果支持某个属性，你可以写一套样式，如果不支持某个属性，你也可以提供另外一套样式作为替补。calc(), 函数用于动态计算长度值。 calc()函数支持 "+", "-", "*", "/" 运算,@media 查询，你可以针对不同的媒体类型定义不同的样式。
---
### 说一下盒模型？
> 盒模型的组成，由里向外content,padding,border,margin.
在IE盒子模型中，width表示content+padding+border这三个部分的宽度
在标准的盒子模型中，width指content部分的宽度
box-sizing的使用
```
  box-sizing: content-box 是W3C盒子模型
  box-sizing: border-box 是IE盒子模型
```
> box-sizing的默认属性是content-box
 ---
 ### BFC
 > BFC （块级格式化上下文），是一个独立的渲染区域，让处于 BFC 内部的元素与外部的元素相互隔离，使内外元素的定位不会相互影响
 触发条件:
- 根元素
- position: absolute/fixed
- display: inline-block / table
 -float 元素
- ovevflow !== visible
规则:
- 属于同一个 BFC 的两个相邻 Box 垂直排列
- 属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠
- BFC 的区域不会与 float 的元素区域重叠
- 计算 BFC 的高度时，浮动子元素也参与计算
---
### 说一下<label>标签的用法
> label标签主要是方便鼠标点击使用，扩大可点击的范围，增强用户操作体验
---
### 页面渲染html的过程？
> 浏览器渲染页面的一般过程：
1. 浏览器解析html源码，然后创建一个 DOM树。并行请求 css/image/js在DOM树中，每一个HTML标签都有一个对应的节点，并且每一个文本也都会有一个对应的文本节点。DOM树的根节点就是 documentElement，对应的是html标签。
2. 浏览器解析CSS代码，计算出最终的样式数据。构建CSSOM树。对CSS代码中非法的语法它会直接忽略掉。解析CSS的时候会按照如下顺序来定义优先级：浏览器默认设置 < 用户设置 < 外链样式 < 内联样式 < html中的style。
3. DOM Tree + CSSOM --> 渲染树（rendering tree）。渲染树和DOM树有点像，但是是有区别的。DOM树完全和html标签一一对应，但是渲染树会忽略掉不需要渲染的元素，比如head、display:none的元素等。而且一大段文本中的每一个行在渲染树中都是独立的一个节点。渲染树中的每一个节点都存储有对应的css属性。
4. 一旦渲染树创建好了，浏览器就可以根据渲染树直接把页面绘制到屏幕上。
5. 以上四个步骤并不是一次性顺序完成的。如果DOM或者CSSOM被修改，以上过程会被重复执行。实际上，CSS和JavaScript往往会多次修改DOM或者CSSOM。
---
### content-visibility
### 图片格式
- GIF
  > 采用LZW算法压缩，无损压缩，尺寸较小，支持透明度和动画，缺点是只存储8位索引，最多只能表示2^8=256种颜色，色彩复杂，细节丰富的图片不适用
- jpg
  > 有损压缩，支持的颜色达 2^24 1600w种，适合色彩丰富，渐变色图片，有损压缩后可以大幅度减小图片尺寸
- png-8
  > 无损压缩，8位索引的位图格式，同等质量下，尺寸更小，非常合适的gif替代格式，唯一缺点是不支持动图
- png-24
  > 无损压缩，支持透明图片，基于直接色的位图，图片质量堪比bmp，尺寸相对更大，因为其品质高，无压缩，适合源文件或者需要二次编辑的图片，虽然png-24能表达更丰富的图片细节，但是并不能替代jpg，因为其转换为jpg后，图片大小是正常jpg的5倍
- webp
  > 新一代图片格式，google开发，相同视觉体验下，图片尺寸缩小了大约30%，支持有损压缩，无损压缩，透明图，动图，理论上可以完全替代png，jpg，gif等图，但目前浏览前并没有全面支持该图。
## 九、coding，算法，源码实现
### 两个变量值交换
```
let a = 1
let b = 2
[a, b] = [b, a]
```
---
### css水平、垂直居中的写法，请至少写出4种？
```
水平居中

行内元素: text-align: center
块级元素: margin: 0 auto
position:absolute +left:50%+ transform:translateX(-50%)
display:flex + justify-content: center

垂直居中

设置line-height 等于height
position：absolute +top:50%+ transform:translateY(-50%)
display:flex + align-items: center
display:table+display:table-cell + vertical-align: middle;
```
---
### 画一条0.5px的直线？
```
height: 1px;
transform: scale(0.5);
```
---
### css画三角形
```
.a{
  width: 0;
  height: 0;
  border-width: 100px;
  border-style: solid;
  border-color: transparent #0099CC transparent transparent;
  transform: rotate(90deg); /*顺时针旋转90°*/
}
<div class="a"></div>
```
### 清除浮动的几种方式，及原理？
```
::after
底部添加空div 设置 clear: both
创建父级 BFC(overflow:hidden)
父级设置高度
```
---
### 遍历所有子节点
```
深度遍历
const DFS = {
    nodes: [],
    do (root) {
        for (let i = 0;i < root.childNodes.length;i++) {
            var node = root.childNodes[i];
            // 过滤 text 节点、script 节点
            if ((node.nodeType != 3) && (node.nodeName != 'SCRIPT')) {
                this.nodes.push(node);
                this.do(node);
            }
        }
        return this.nodes;
    }
}
console.log(DFS.do(document.body));

广度遍历
const BFS = {
    nodes: [],
    do (roots) {
        var children = [];
        for (let i = 0;i < roots.length;i++) {
            var root = roots[i];
            // 过滤 text 节点、script 节点
            if ((root.nodeType != 3) && (root.nodeName != 'SCRIPT')) {
                if (root.childNodes.length) children.push(...root.childNodes);
                this.nodes.push(root);
            }
        }
        if (children.length) {
            var tmp = this.do(children);
        } else {
            return this.nodes;
        }
        return tmp;
    }
}
console.log(BFS.do(document.body.childNodes));
```