title: Webpack实现原理探索
speaker: 梁少峰
url: https://github.com/youngwind/fake-webpack
transition: slide3
theme: moon
usemathjax: yes


[slide]

# Webpack 实现原理探索
## 从代码打包说起
### [@梁少峰](https://github.com/youngwind)

[slide]
# 目标
---
## [复用 CommonJS 规范的代码](https://github.com/webpack/webpack/tree/2e1460036c5349951da86c582006c7787c56c543#goal)

[slide]
```js
// example.js
let a = require('a');
let b = require('b');
let c = require('c');
a();
b();
c();
```
```js
// a.js
module.exports = function () {
    console.log('a')
};
```
```js
// b.js
module.exports = function () {
    console.log('b')
};
```
```js
// c.js
module.exports = function () {
    console.log('c')
};
```

[slide]
# bundle.js 内部构成
* 一个 IIFE
* 一段模板
* 以数字为键值的 modules 集合

---
### Webpack 本质 → 字符串拼接工具


[slide]
# 依赖分析
* 要知道 example 依赖 a、b 和 c → [esprima](http://esprima.org/demo/parse.html) {:&.moveIn}
* CommonJS 特性：加载时执行 → 深度遍历

[slide]
# 依赖分析
![depTree](/img/webpack/depTree.png)

[slide]
# 模块路径逐层往上查找
---
### 如何知道模块 c 位于 node_modules 文件夹中？

[slide]
# 以数字为模块 id
---
### 要注意从后往前替换

[slide]
# 延伸
1. [webpack源码学习系列之二：code-splitting（代码切割）](https://github.com/youngwind/blog/issues/100)
2. [webpack源码学习系列之三：loader 机制](https://github.com/youngwind/blog/issues/101)

[slide]
# 谢谢