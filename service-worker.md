title: 浏览器缓存：历史与未来
speaker: 梁少峰
url: https://github.com/youngwind/blog/issues/113
files:
transition: move
theme: moon
usemathjax: yes

[slide]

# 浏览器缓存的历史与未来
## 如何实现离线应用
### [@梁少峰](https://github.com/youngwind/blog/issues/113)

[slide]
# 思路
1. Demo
1. 浏览器缓存
3. CacheStorage
2. Application Cache（应用缓存）
4. Web Worker
5. Service Worker
6. 代码实现

[slide]
# Demo 效果
![demo](/img/service-worker/demo.gif)

[slide]
# 浏览器缓存
* 分类（Header 缓存） {:&.fadeIn}
    * [强缓存](http://coderlt.coding.me/2016/11/21/web-cache/)
    * [协商缓存 304](http://www.cnblogs.com/wonyun/p/5524617.html)
* 缺点
    * 缓存不可编程
    * 离线无法访问
        * HTML 能缓存吗？
        * 恐龙游戏


[slide]
# CacheStorage
![cachestorage](/img/service-worker/cache.png)

[slide]
# 如何才能做到离线？
* 缓存 HTML → 解决版本更新问题
* 告诉浏览器，我不想玩恐龙游戏

[slide]
# Application Cache
```html
<html manifest="example.appcache">
  ...
</html>
```
```
# example.appcache
CACHE MANIFEST
index.html
cache.html
style.css
image1.png
```
[为什么app cache没有得到大规模应用？它有哪些硬伤吗？](https://www.zhihu.com/question/29876535)


[slide]
# [Web Worker](http://mdn.github.io/simple-web-worker/)
* 独立的 JS 进程
* 可以与 UI 进程并行
* 不会常驻浏览器
* 页面之间不能共享

[slide]
# Service Worker
* ~~独立的 JS 进程~~
* ~~可以与 UI 进程并行~~
* 会常驻浏览器
* 同源多个页面之间可以共享

[slide]
# 离线应用的架构
![sw](/img/service-worker/architecture.jpg)

[slide]
# 谢谢