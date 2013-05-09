---
layout: post
title: "jekyll集成评论以及相关文章"
description: "jekyll集成无觅相关文章以及评论"
category: others 
tags: [博客搭建, jekyll, 无觅相关文章, 友言评论]
---
{% include JB/setup %}

Jekyll安装完毕后，自带了一些社交化的小东东，类似评论啊之类的。博主的强迫症果断又开始爆发，于是踏上了折腾的道路~

在此十分感谢[煲仔饭](http://ivoryxiong.org/tutorial/2012/10/30/jekyll-wumii-related-posts/)的分享，让博主这个盲流(文盲+流氓)少了很多探索， 同时也要感谢一下无觅以及友言提供的服务(这坑爹的不是*广告*啊， 这是凑字数用的了，不然这篇文章也太短了，男人果断不能忍啊！！！)。

***

### 相关文章

相关文章这个东西，杀人利器。你过来，我过去，是吧，流量什么的就有啦~妈妈再也不用担心我的点击率了(好吧，那是吹牛的，目前访问率为0的说→ →)。

Jekyll-Bootstrap默认貌似没有相关文章的插件，果断参照上文提及的步骤走起，集成无觅。在集成的时候最好注意几点：  

  * 最好在`_includes\custom`目录下统一存放自定义的模板，因为已有的模板中默认回去这里找用户自定义的模板。
  * js中的变量使用Jekyll的全局变量以及Liquid脚本走起，楼主的大致如下:
  		
		var wumiiPermaLink = "{{site.production_url}}{{page.url}}"; 
    	var wumiiTitle = "{{page.title}}"; 
    	var wumiiTags = "{% for tag in page.tags %} {{tag}}, {% endfor %}"; 

  接下来就是提交，push，洗洗睡了。。。

### 评论

像楼主这种打酱油的，几乎就不会使用这种功能，除非是用于`楼主好人，一生平安，邮箱xxx@xxx.xxx`之类的时候(少年，不要想多了哈→ →)。但是考虑到未来说不定有同学来*约架*或者PLMM来*约x*，果断还是把评论功能加上了。BTW， JB(Jekyll-Bootstrap， 想些啥呢。。。)默认带了评论系统的，但是考虑到使用的貌似foreigner居多，约那个啥的成本过高，果断使用了国内的*友言*， 这个真心不是广告。。。

简要步骤：  

  * 去[友言](http://www.uyan.cc/)获取*通用*代码，最好注册一个方便管理。
  * `_includes\custom`目录下新建一个`comments`文件(是的，你没看错，没有后缀哈)， 加入刚才得到的代码。
  * 去_config.yml文件里面配置一下  
    ![_config.yml插件配置](/assets/images/2013-05/jb-comments-config.png)  
    就是把`comments`的`provider`属性由`disqus`改成`custom`
  * git提交然后push。

**注意**，如果你使用了`jekyll --server`在本地调试，那么请使用`ip:port`的方式访问，**不要使用**`localhost:port`，否则评论插件无法显示的！



