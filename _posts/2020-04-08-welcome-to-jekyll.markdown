---
layout: post
title:  "hello blog"
date:   2020-04-08 16:57:21 +0800
categories: blog
tags: jekyll,git_page,markdown
---
写博客的平台有很多.  
[知乎](https://www.zhihu.com/)  
[博客园](https://www.cnblogs.com/)  
[微博](https://www.weibo.com/)  
如果希望自己管理博客,推荐使用[GitHub Pages](https://pages.github.com/),免费,快速,门槛低.

# 首先创建GitHub Pages repository  
Create repo:  
1. 名称格式username.github.io,username为自己git账号的username.
2. 普通账号repo必须public
3. 勾选(Initialize this repository with a README)  

创建完成,在repo的settings页面找到GitHub Pages相关设置,可以看到Your site is published at https://username.github.io/,点击链接,页面已经成功发布,可以访问了.  
上面已经创建好了博客repo,如果只是用作发布个人简历等简单用途,那在根目录修改md文件既可.
{% highlight markdown %}
# 个人信息

 - xx/男/1999 
 - 本科/xx大学计算机系 
 - 工作年限：1年  
 - Github：http://github.com/xx  

 - 期望职位：架构师  
 - 期望薪资：100w  
 - 期望城市：不限  
{% endhighlight %}  

# 搭建博客工具
如果希望搭建功能完整的bolg.比如这样[阮一峰](http://www.ruanyifeng.com/blog/),那可以用静态博客工具[Jekyll](http://jekyllcn.com/),这是GitHub推荐的工具.同样好用的还有[Hugo](https://www.gohugo.org/),[gridea](https://gridea.dev/).如果使用Jekyll.需要安装ruby环境.   

Jekyll安装需要ruby环境  
 - 第一步安装[Ruby installer](https://rubyinstaller.org/)
 - 第二步安装[RubyGems](https://rubygems.org/pages/download),解压后目录执行命令 ruby setup.rb
 - 然后就可以安装Jekyll了:gem install jekyll

 Jekyll使用     
 jekyll new testblog    
 jekyll server  
 如果报错版本不一致需要使用: bundle exec jekyll server  