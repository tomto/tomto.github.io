---
layout: post
title:  "asp中使用console-log"
date:   2020-04-17 11:18:21 +0800
categories: debug
tags : vs,debug
---

# 在某些不适合断点调试的场景,会希望和js一样把console.log打在output输出框.

这时候可以使用**Trace**和**Debug**  
Trace会同时在Debug,Release模式起作用,而Debug只作用在Debug模式下输出.  
{% highlight C# %}
Trace.WriteLine();

Debug.WriteLine();

{% endhighlight %}  

f5运行.如果未生效.右键project=>propertis=>Build=>Define DEBUG constant or Define Trace constant  
勾选上面check box重启vs,即可解决.


