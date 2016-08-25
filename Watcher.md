title: 如何实现一个watch库: 从vue源码说起
speaker: 梁少峰
url: https://github.com/youngwind/bue
files: /css/watch.css
transition: slide3
theme: moon
usemathjax: yes


[slide]

# 如何实现一个watch库
## 从vue源码说起
### [@梁少峰](https://github.com/youngwind)

[slide]
# 概要
----
* Demo演示
* 基本原理
* 难点
* 延伸思考

[slide]
# Demo演示
<img src='/img/watch/demo.gif'>

[slide]
# Demo代码
```
<div id="app">
    <p>姓名: {{name}}</p>
    <p>年龄: {{age}}</p>
</div>
```

```
import Bue from '../src/index';

const app = new Bue({
    el: '#app',
    data: {
        name: 'youngwind',
        age: 24
    }
});
```

[slide]
# 目标分析
* Vue?❌ {:&.moveIn}
* 动态绑定? ❌
* 对象变动监听(watch库)? ✅

[slide]
# 千里之行
```
function Observer(data) {
    this.data = data;
    for (let key in data) {
        let val = data[key];
        if (!data.hasOwnProperty(key)) return;
        Object.defineProperty(this.data, key, {
            enumerable: true,
            configurable: true,
            get: function () {
                console.log('你访问了' + key);
                return val
            },
            set: function (newVal) {
                if (newVal === val) return;
                val = newVal
                console.log('你设置了' + key + ' 新的' + key + ' = ' + newVal);
            }
        })
    }
}

let data = {
    name: "youngwind",
    age: 24
};
const person = new Observer(data);
person.data.name = "liangshaofeng";   // 你设置了name,新的值=liangshaofeng
```

[slide]
# 困难重重
* 对象的属性依然是一个对象 -> 递归  {:&.moveIn}
* 如何监听数组?  -> 重写push、pop等prototype方法


[slide]
# 奇技淫巧
```
const arrayAugmentations = [];

['push', 'pop', 'shift', 'unshift'].forEach((method)=> {
    let original = Array.prototype[method];
    Object.defineProperty(arrayAugmentations, method, {
        enumerable: true,
        configurable: true,
        writable: true,
        value: function () {
            console.log('我被改变啦!');
            let result = original.apply(this, arguments);
            return result;
        }
    });

});

module.exports = arrayAugmentations;
```

```
import arrayAugmentations from '../observer/array-augmentations';
function Observer(data) {
    // ...
    if(Array.isArray(data)){
       data.__proto__ = arrayAugmentations;
    }
    // ...
}
```

[slide]
# 还有什么?
* 观察者模式 {:&.moveIn}
```
Observer.prototype.emit = function (event, path, val) {
    this._cbs = this._cbs || {};
    let callbacks = this._cbs[event]
    if (!callbacks) return;
    callbacks = callbacks.slice(0);
    callbacks.forEach((cb, i) => {
        callbacks[i].apply(this, arguments)
    });
};
```

* 事件传播
```
Observer.prototype.notify = function (event, path, val) {
    this.emit(event, path, val);
    let parent = this.parent;
    if (!parent) return;
    let ob = parent.ob;
    ob.notify(event, path, val);
};
```

* 源码
https://github.com/youngwind/bue

[slide]
# 谢谢