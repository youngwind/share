title: 如何实现一个异步模块加载器
speaker: 梁少峰
url: https://github.com/youngwind/fake-requirejs
files:
transition: move
theme: moon
usemathjax: yes

[slide]

# 如何实现异步模块加载器
## 以requireJS为例
### [@梁少峰](https://github.com/youngwind/blog/issues/98)

[slide]
# Why
1. [模块化发展历史](https://huangxuan.me/js-module-7day/#/)
2. amd、commonjs、cmd规范之争

[slide]
# Module原型设计
```js
Module.id       // 模块id
Module.name     // 模块名字
Module.src      // 模块的真实的uri路径
Module.dep      // 模块的依赖
Module.cb       // 模块的成功回调函数
Module.errorFn  // 模块的失败回调函数
Module.STATUS   // 模块的状态(等待中、正在网络请求、准备执行、执行成功、出现错误……)

Module.prototype.init           // 初始化,用来赋予各种基本值
Module.prototype.fetch          // 通过网络请求,获取模块
Module.prototype.analyzeDep     // 分析、处理模块依赖依赖(这个是难点)
Module.prototype.execute        // 运算该模块,得到export
```

[slide]
# 依赖分析与处理
```
// a.js
define(function () {
    var hi = function () {
        console.log('hi');
    };
    return {
        hi: hi
    }
});
```
```
// b.js
define(['c'], function (c) {
    var goodbye = function () {
        console.log('goodbye');
    };
    c.show();
    return {
        goodbye: goodbye
    }
});
```

[slide]
# 依赖分析与处理

```
// c.js
define(function () {
    var show = function () {
        console.log('show');
    };
    return {show}
});
```

```js
// 入口main.js
require(['a', 'b'], function (a, b) {
    a.hi();
    b.goodbye();
}, function () {
    console.error('Something wrong with the dependent modules.');
});
```

```html
<script src="../require.js" data-main="main"></script>
```

[slide]
![map](/img/requirejs/map.png)

[slide]
# 共同的难题
1. [Commonjs和ES6的循环依赖](http://www.ruanyifeng.com/blog/2015/11/circular-dependency.html)
2. [seajs的循环依赖](https://github.com/seajs/seajs/issues/732)
3. [requirejs的循环依赖](http://requirejs.cn/docs/api.html#circular)

[slide]
# 例子
```js
// a.js
define(['b'],function (b) {
    var hi = function () {
        console.log('hi');
    };
    b.goodbye();
    return {hi: hi}
});
```
```js
// b.js
define(['require', 'a'], function (require) {
    var goodbye = function () {
        console.log('goodbye');
    };
    require(['a'], function (a) {
        a.hi();
    });
    return {goodbye: goodbye}
});
```

[slide]
# 解决方法
```js
// 引入新的类: Task(任务)
 function Task(dep, cb, errorFn) {
    this.tid = ++tid;
    this.init(dep, cb, errorFn);
}

// Task类继承于Module类
Task.prototype = Object.create(Module.prototype);
```
```js
// 每一次调用require,相当于新建一个Task
require = function (dep, cb, errorFn) {
    let task = new Task(dep, cb, errorFn);
    task.analyzeDep();
};
```

[slide]
# 参考资料
1. [源码](https://github.com/youngwind/fake-requirejs)
1. [学着写一个异步模块加载器](http://www.w2bc.com/article/169565)
2. [动手实现一个简单的浏览器端js模块加载器](http://www.jianshu.com/p/0505b1718dab)
3. [requireJS源码学习--叶小钗](http://www.cnblogs.com/yexiaochai/p/3632580.html)
4. [别人实现的MyRequireJS](https://github.com/foio/MyRequireJS)

[slide]
#谢谢
