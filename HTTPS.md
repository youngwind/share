title: 图解 HTTPS
speaker: 梁少峰
url: https://github.com/youngwind/blog/issues/108
transition: slide3
theme: moon
usemathjax: yes

[slide]

# 图解 HTTPS
## Charles 为什么能捕获 HTTPS 请求
### [@梁少峰](https://github.com/youngwind/blog/issues/108)

[slide]
# HTTP 为什么不安全？
![不安全的 HTTP](/img/https/image1.png)

[slide]
# 密码学是安全的基石
# 数学是密码学的基石

[slide]
# 古代的对称加密
## [凯撒密码](https://zh.wikipedia.org/wiki/%E5%87%B1%E6%92%92%E5%AF%86%E7%A2%BC)
```
明文字母表：ABCDEFGHIJKLMNOPQRSTUVWXYZ
密文字母表：DEFGHIJKLMNOPQRSTUVWXYZABC

向右偏移三位：CAT => FDW
```
1. 每位将军手上的密钥都是一样的
2. 加密和解密可以相互反推
3. 一旦某一位将军叛逆，全体通信便会被攻破

[slide]
# 现代的对称加密
## [XOR 运算](http://www.ruanyifeng.com/blog/2017/05/xor.html)
```
1010 ^ 1111 = 0101
0101 ^ 1111 = 1010

=> A ^ B ^ B = A
```
应用：[DES 算法](https://bignerdcoding.com/archives/31.html)
[slide]
# 非对称加密
[不恰当的例子](https://www.zhihu.com/question/33645891/answer/192604856)
```
123 * 91 = 11193
11193 * 11 = 123123
=> 任何一个三位数乘以 1001（91*11） 后，末三位都不变
=> 加密和解密可以用不同的钥匙
=> 算出 91 * 11 = 1001 很容易，算出 1001 = 91 * 11 很不容易
```

[大整数质数分解：RSA算法](http://www.ruanyifeng.com/blog/2013/07/rsa_algorithm_part_two.html)

[slide]
# 对称 VS 非对称
![对称加密 VS 非对称加密](/img/https/image2.png)

[slide]
# 从简单的开始
![对称加密应用](/img/https/image3.png)

[slide]
# 兄弟齐上阵
![结合对称加密和非对称加密](/img/https/image4.png)

[slide]
# 道高一尺，魔高一丈
![中间人攻击](/img/https/image5.png)

[slide]
# 胜利最终是人民的
![数字证书](/img/https/image6.png)

[slide]
# 证书、根证书（证书中心），CA
![证书与证书中心](/img/https/image8.png)

[slide]
# 随便安装根证书的后果
![Charles](/img/https/image7.png)

[slide]
# 谢谢

[slide]
# 参考资料
1. [关于互联网流量劫持分析及可选的解决方案](https://my.oschina.net/leejun2005/blog/614612), By xrzs
1. [密码学笔记](http://www.ruanyifeng.com/blog/2006/12/notes_on_cryptography.html), By 阮一峰
2. [对称加密算法 VS 非对称加密算法](http://blog.loveyoung.me/2016/02/19/%E7%99%BD%E8%AF%9D%E8%A7%A3%E9%87%8A-%E5%AF%B9%E7%A7%B0%E5%8A%A0%E5%AF%86%E7%AE%97%E6%B3%95-%E9%9D%9E%E5%AF%B9%E7%A7%B0%E5%8A%A0%E5%AF%86%E7%AE%97%E6%B3%95.html), By loveyoung
3. [密码技术系列 Part 1 - 对称加密](http://bignerdcoding.com/archives/31.html), By BigNerdCoding
4. [如何用通俗易懂的话来解释非对称加密](https://www.zhihu.com/question/33645891/answer/192604856), By ThreatHunter
4. [XOR 加密简介](http://www.ruanyifeng.com/blog/2017/05/xor.html), By 阮一峰

[slide]
# 参考资料
7. [RSA算法原理（一）](http://www.ruanyifeng.com/blog/2013/06/rsa_algorithm_part_one.html), By 阮一峰
6. [RSA算法原理（二）](http://www.ruanyifeng.com/blog/2013/07/rsa_algorithm_part_two.html), By 阮一峰
7. [数字签名是什么？](http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html), By 阮一峰
7. [看完还不懂HTTPS我直播吃翔](http://blog.csdn.net/winwill2012/article/details/71774469), By winwill2012  🌟 🌟 🌟
8. [关于HTTPS，你需要知道的全部](http://www.jianshu.com/p/fb6035dbaf8b), By rushjs
12. [深入HTTPS系列一（HTTP&HTTPS）](http://www.jianshu.com/p/a677fecec927), By muice
10. [HTTPS为什么安全 &分析 HTTPS 连接建立全过程](http://www.jianshu.com/p/0d8575b132a8), By kaitoulee
9. [SSL/TLS协议运行机制的概述](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html), By 阮一峰
11. [浅谈Charles抓取HTTPS原理](http://www.jianshu.com/p/405f9d76f8c4), By rushjs
13. [Nodejs创建HTTPS服务器](http://blog.fens.me/nodejs-https-server/), By 张丹