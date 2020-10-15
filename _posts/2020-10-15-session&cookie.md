---
layout: post
title:  "Session和Cookie"
date:   2020-10-15 13:29:21 +0800
categories: blog
tags : session,cookie
---

Cookie 和 Session 是什么,并且有什么区别？   
感觉这个问题很简单,脱口而出.这里简单整理一下自己的理解和思考.     

# 第一层:Session和Cookie的产生和定义
因为Http是无状态协议,所以我登录一个站点以后,想要看站点内其他的页面或者记住一些用户设置,那就必须要有一个地方能把我的登录信息记录下来,这时候Session和Cookie就产生了,他们分别在客户端(Cookie)和服务器端(Session)存储数据.   

什么是 Cookie   
HTTP Cookie（也叫 Web Cookie或浏览器 Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据,它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上.通常,它用于告知服务端两个请求是否来自同一浏览器,如保持用户的登录状态.Cookie 使基于无状态的 HTTP 协议记录稳定的状态信息成为了可能.
Cookie 应用：
会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
个性化设置（如用户自定义设置、主题等）
浏览器行为跟踪（如跟踪分析用户行为等）  
什么是 Session  
Session 代表着服务器和客户端一次会话的过程.Session 对象存储特定用户会话所需的属性及配置信息.这样,当用户在应用程序的 Web 页之间跳转时,存储在 Session 对象中的变量将不会丢失,而是在整个用户会话中一直存在下去.当客户端关闭会话,或者 Session 超时失效时会话结束.   

# 第二层:Session和Cookie怎么工作的 
用户第一次请求服务器的时候,服务器根据用户提交的相关信息,创建创建对应的 Session ,在返回请求的Set-Cookie内会有SessionID,浏览器接收到服务器返回的 SessionID 信息后,会将此信息存入到 Cookie 中,同时 Cookie 记录此 SessionID 属于哪个域名.    
当用户第二次访问服务器的时候,请求会自动判断此域名下是否存在 Cookie 信息,如果存在自动将 Cookie 信息也发送给服务端,服务端会从 Cookie 中获取 SessionID,再根据 SessionID 查找对应的 Session 信息,如果没有找到说明用户没有登录或者登录失效,如果找到 Session 证明用户已经登录可执行后面操作.  
SessionID 是连接 Cookie 和 Session 的一道桥梁,大部分系统也是根据此原理来验证用户登录状态.   
如果Cookie在浏览器端被禁用,Session是否能正常工作?   
禁用Cookie那就把SessionID存在客户端别的地方,然后通过每次请求url带上SessionID即可.Cookie和Session只是常用方案的一种,理论上单独使用Cookie或Session都是可行的.但实际中一般不会单独使用,如果全部用Cookie,第一数据存在客户端是不安全的,第二数据量大Cookie是有空间大小限制的(≈4kb).   

# 第三层Session和Cookie的实现是否完美
业务的负责会增加Session内数据的大小,访问量的提高会增加Session的数量,这时候一台服务器不够用了.加机器(Cluster)!,那Session数据怎么办呢.要么根据用户IP让同一个用户只访问一个机器,要么服务器随时同步Session数据.这两种方式都不完美.前一种机器A挂掉Session数据就没了,后一种Session数据在任何节点的变动都需要广播到所有服务器,这并没有消减服务器的压力.这时候就要用到缓存等中间件来把Session数据单独出来统一管理.甚至Session存储也可以上集群,增加可靠性.但是感觉怎么弄都还是很麻烦,能不能换个思路.服务器为什么要保存这些信息呢, 只让每个客户端去保存该多好？     
![RUNOOB]({{site.url}}/assets/img/1350514-20180504123036062-1920411426.png)

# 第四层 Token

Token机制和Cookie和Session的使用机制类似.客户端使用用户名跟密码请求登录,验证成功后,服务端会签发一个Token,再把这个Token发送给客户端,客户端收到 Token 以后可以把它存储起来,比如放在localStorage中,客户端每次向服务端请求资源的时候需要带着服务端签发的Token,服务端收到请求,然后去验证客户端请求里面带着的 Token.   

![RUNOOB]({{site.url}}/assets/img/6943526-f6773b6740bd67f9.png)

Token的特性:    
- 支持跨域访问: Cookie是不允许垮域访问的,token支持
无状态：token无状态,session有状态的
- 去耦: 不需要绑定到一个特定的身份验证方案.Token可以在任何地 方生成,只要在 你的API被调用的时候,你可以进行Token生成调用即可.
- 更适用于移动应用: Cookie不支持手机端访问的
- 安全性:请求中发送token而不再是发送cookie能够防止CSRF(跨站请求伪造).
- 基于标准化: 你的API可以采用标准化的 JSON Web Token (JWT). 这个标准已经存在 多个后端库（.NET, Ruby, Java,Python, PHP）和多家公司的支持（如： Firebase,Google, Microsoft）