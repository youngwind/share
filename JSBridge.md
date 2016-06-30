title: JSBridge实现原理:以toast为例
speaker: 梁少峰
url: https://github.com/youngwind/android-demo
files: /css/jsbridge.css
transition: slide3
theme: moon
usemathjax: yes

[slide]

# JSBridge实现原理探索
## 以toast为例
### [@梁少峰](https://github.com/youngwind) 

[slide]
# 概要 
----
* Demo演示
* 思路分析
* 代码实现
* 延伸思考
* 参考资料

[slide]
# Demo演示
<img src='/img/jsbridge-demo.gif' class="demo">
<img src='/img/jsbridge-demo-qr.png' class="demo-qr">

[slide]
# 思路分析
* 难点: 如何让web和安卓进行沟通? {:&.moveIn}
* 例子: 中国人与美国人沟通(动作监听 && 协议制订)
* webview监听事件: onJsAlert
* 双方制定协议: ywjs://toast?你想要传送的消息

[slide]
# 代码实现
* onJsAlert  {:&.moveIn}
* onJsConfirm
* onJsPrompt
* shouldOverrideUrlLoading
* addJavascriptInterface

[slide]
# 延伸思考
* bainuo://component?compid=组件包&comppage=组件页面 {:&.moveIn}
* 回调函数问题
* Java调js方法
* ios版本

[slide]
# 参考资料 
* http://blog.csdn.net/sbsujjbcy/article/details/50752595
* http://rensanning.iteye.com/blog/2043049
* https://github.com/pedant/safe-java-js-webview-bridge
* http://blog.csdn.net/leehong2005/article/details/11808557
* http://blog.csdn.net/jackyhuangch/article/details/8310033
* http://droidyue.com/blog/2014/07/09/override-javascript-alert-in-android/index.html
* Demo源码: https://github.com/youngwind/android-demo

[slide]
# 谢谢
