title: 再谈回调、异步与生成器
speaker: 梁少峰
url: https://github.com/youngwind/bue
files:
transition: move
theme: moon
usemathjax: yes

[slide]

# 再谈回调、异步与生成器
## 以实现co库为例
### [@梁少峰](https://github.com/youngwind/blog/issues/96)

[slide]
# 概要
----
* 回调与事件循环
* promise与micro-task
* 为generator正名
* 如何实现co库

[slide]
# 回调的弊端
```js
doA(function(){
  doB();
  doC(function() {
    doD();
  });
  doE();
});
doF();
```
* 代码缩进造成金字塔（小问题） {:&.moveIn}
* 嵌套的书写方式与人类顺序大脑思考方式相违背（大问题）
* 前后操作被硬编码绑定在一起，代码变得僵硬，难以维护（大问题）

[slide]
# 事件循环
```js
console.log(1);
setTimeout(() => {
    console.log(2);
},1000);
console.log(3);
```

```js
console.log(1);
Ajax().done((res) => {
  console.log(res);
});
console.log(2);
```
* JS本身，从来没有真正內建直接的异步概念，直到ES6的出现。 {:&.moveIn}


[slide]
# 没想到你是这样的promise
```js
setTimeout(() => {console.log(4);},0);
new Promise((resolve) => {
    console.log(1);
    resolve();
    console.log(2);
}).then(() => {
    console.log(5);
});
console.log(3); // 问题: 为什么输出是12354,而不是12345
```


[slide]
# macro-task VS micro-task
> 异步任务的两种分类。
在挂起任务时，JS 引擎会将所有任务按照类别分到这两个队列中，首先在 macrotask 的队列（这个队列也被叫做 task queue）中取出第一个任务，
执行完毕后取出 microtask 队列中的所有任务顺序执行；
之后再取 macrotask 任务，周而复始，直至两个队列的任务都取完。
1. macro-task: script（整体代码）, setTimeout, setInterval, setImmediate, I/O, UI rendering
2. micro-task: process.nextTick, Promises（这里指浏览器实现的原生 Promise）, Object.observe, MutationObserver

出处:[图灵社区:Promise/A+规范](http://www.ituring.com.cn/article/66566)



[slide]
# 更加复杂的promise
```js
setTimeout(() => {console.log(4);},0);
new Promise((resolve) => {
    console.log(1);
    resolve();
    console.log(2);
}).then(() => {
    console.log(5);
    new Promise((resolve) => {
        console.log(6);
        resolve();
        console.log(7);
    }).then(() => {
        console.log(8);
        setTimeout(() => {console.log(9);},0)
    })
});
console.log(3); // 输出: 123567849
```





[slide]
# 顾名思义
```js
function* foo(x){
  let y = x * (yield);
  return y;
}

let it = foo(6);
let res = it.next();  // res是什么
res = it.next(7);     // res是什么
```
* generator → 生成(iterator) → 输入输出 {:&.moveIn}
* yield → 让道/产出 → 函数中断重启

[slide]
# 实例-回调实现
```js
setTimeout(() => {
  console.log(1, new Date());
  setTimeout(() => {
    console.log(2, new Date());
    setTimeout(() => {
       console.log(3, new Date());
    },2000)
  }, 1000);
},1000);
```

[slide]
# 实例-promise实现
```js
function p(time){
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(new Date());
    }, time)
  });
}

p(1000).then((data) => {
  console.log(1, data);
  return p(1000);
}).then((data) => {
  console.log(2, data);
  return p(2000);
}).then((data) => {
  console.log(3, data);
})
```

[slide]
```js
function p(time) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(new Date());
    }, time)
  });
}

co(function* delay(){
  let time1 = yield p(1000);
  console.log(1, time1);
  let time2 = yield p(1000);
  console.log(2, time2)
  let time3 = yield p(2000);
  console.log(3, time3);
})

function co(gen){
  let it = gen();
  next();
  function next(arg){
    let ret = it.next(arg);
    if(ret.done) return;
    ret.value.then((data) => {
      next(data)
    })
  }
}

```

[slide]
# 实例-async与await实现
```js
function p(time) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
       resolve(new Date());
    }, time)
  });
}

(async function(){
  let time1 = await p(1000);
  console.log(1, time1);

  let time2 = await p(1000);
  console.log(2, time2)

  let time3 = await p(2000);
  console.log(3, time3);
})()
```
[slide]
# 谢谢