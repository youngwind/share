title: 如何实现动态数据绑定: 从vue源码说起
speaker: 梁少峰
url: https://github.com/youngwind/bue
files: /css/watch.css
transition: move
theme: moon
usemathjax: yes


[slide]

# 如何实现动态数据绑定
## 从vue源码说起
### [@梁少峰](https://github.com/youngwind)

[slide]
# 概要
----
* Demo
* 三步走

[slide]
# Demo代码
```html
<div id="app">
    <p>姓名: {{user.name}}</p>
    <p>年龄: {{user.age}}</p>
</div>
```
```js
import Bue from 'bue';
const app = new Bue({
    el: '#app',
    data: {
        user: {
            name: 'youngwind',
            age: 24,
            school: 'BUPT'
        }
    }
});

app.$watch('user.name', function () {
    console.log('watch住了user.name');
});
```

[slide]
# Demo效果
<img src='/img/binding/demo.gif'>

[slide]
# 第一步: 简单粗暴
```js
this.observer.on('set', this._compile);   // 注册监听事件
```
```js
exports._compile = function () {
    fragment = document.createDocumentFragment();
    this._compileNode(this.$template);
    this.$el.parentNode.replaceChild(fragment, this.$el);  //用fragment替换原先的DOM
};
```

```js
exports._compileNode = function (node) {
    switch (node.nodeType) {
        case 1:
            this._compileElement(node);   // 编译元素节点
            break;
        case 3 :
            this._compileText(node);      // 编译文本节点
            break;
        default:
            return;
    }
};
```

[slide]
# 实现效果
<img src='/img/binding/bug.gif'>

[slide]
# 问题
* 问题: 任意属性发生变化,都会触发重新渲染整个DOM {:&.moveIn}
* 目标: 只重新渲染变化数据对应的DOM
* 思路: 将DOM节点和关联的数据一一映射起来 → Directive(指令)



[slide]
# 第二步: Directive
```
// 指令构造函数
function Directive(el, expression, vm) {
    this.expression = expression;       // 指令表达式，例如 "name"
    this.el = el;                       // 指令对应的DOM元素
    this.vm = vm;                       // 指令所属bue实例
    this.update();
}

Directive.prototype.update = function () {
    this.el.nodeValue = this.vm.$data[this.expression];
};
```
```js
this._directives.push(new Directive(node, expression, this));
```
```js
    this._directives.forEach((directive) => {
        if (directive.expression !== path) return;
        directive.update();
    });
```

[slide]
# 实现效果
<img src='/img/binding/demo2.gif'>

[slide]
# 问题
* _directive数组循环性能太低 → Binding {:&.moveIn}
* 无处安身的$watch → Watcher

[slide]
# 第三步: Binding、Watcher
### 此处代码太复杂, 已被屏蔽😁

[slide]
# 谢谢

