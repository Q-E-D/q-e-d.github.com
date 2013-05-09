---
layout: post
title: "小记博客搭建成功"
description: "Jekyll windows github page 个人主页 博客"
category: others
tags: [博客搭建]
---
{% include JB/setup %}

在此**劝告兼警告**想在`windows`下搭建Jekyll + windows + github page的同学，洗洗睡吧→ →  
各种坑爹的设定让你无法淡定，各种环境不兼容啊、乱码啊等等，此处省略呵呵一万字。  
不过如果你依然坚持，我只能说，哥们，加油，前面有无数的同学铺好了路，大胆的折腾吧~

***

### 步骤
搭建的主要过程基本参考[利用Jekyll搭建个人博客](http://www.mceiba.com/develop/jekyll-introduction.html)这篇文章，在此再次感谢下博主以及博文下方的各种回复。同时，还参考了[Zero to Hosted Jekyll Blog in 3 Minutes](http://jekyllbootstrap.com/)这篇文章，虽然是E文，但是十分精炼，同学们可以大胆去强势围观（自从有了有道词典，妈妈再也不用担心我的E文了。。）。在文章开始部分`2 - Install Jekyll-Bootstrap`里面，新手同学注意  

	git remote set-url origin git@github.com:USERNAME/USERNAME.github.com.git  
这段代码中使用了GIT的协议，如果你使用https认证而非ssh认证，注意更改格式为https的！
![git仓库地址切换](/assets/images/2013-05/github-link.png)

在搭建环境的过程中，尤其是python部分，为了语法高亮安装`pygments`时，各种报错被楼主无视了，原谅这个可怜的python盲。不过最后竟然没啥影响，看来楼主的RP还是很不错的^_^

### 主题
接下来是配置主题，这个没啥好说的，新手的话可以使用[现成主题](http://themes.jekyllbootstrap.com/)，步骤十分简单。当然如果你是高玩的话，可以自己写模板、写主题，如果有大能写了十分NB的主题， 欢迎分享~

### 汉化
开始博客有几个默认栏目，Archive、Categories、Pages、Tags都是用于代码演示的，大家可以学习一下。有强迫症要将其汉化的的同学，此处提供3个方法：  

	* 在模板文件中去除栏目生成逻辑代码，直接写死。。。
	* 在`_config.yml`文件中定义变量，然后修改模板文件中生成逻辑的代码。
	* 修改文件的title值

以默认主题为例，前两种方法提到的代码位于`_includes\themes\twitter\default.html`文件里面：  
  
	{% assign pages_list = site.pages %}
    {% assign group = 'navigation' %}
最简单的就是修改title属性, 以tags.html为例:  

	---
	layout: page
	title: 标签
	header: Posts By Tag
	group: navigation
	---
修改成这样，成功完成汉化。

### 图片

新手上路。。在项目assets下建立一个图片存放目录，然后再md文件中一绝对路径引用了(`/assets/images/..`)。。。


### 杂项
  * Jekyll中博文默认分page以及post，前者用于做`关于我`之类的页面，后者就是主要的博文了。具体的发布方法很多，简单的使用命令行：  
	
	rake page name='about.md'  

	rake post title='2013-05-08-hello-world'  

  切记命令行上等号附近别乱加空格，不然。。。。。还有，各种配置文件以及MD文件中也不要乱使用空格，不然会有各种*惊喜*等着亲~

  * 文件名不要用中文，否则又是各种悲剧等待着你。。。。。

  * 文件中如果使用了中文， 那文件格式**必须使用**`UTF-8无BOM`格式，否则就悲剧了。

  * 命令行启动时， 有过有中文需要进行配置。传说在git bash环境下只要配置`LC_ALL=en_US.UTF-8` 和 `LANG=en_US.UTF-8`两个环境变量，但是楼主测试未果，原因未知。。在普通的`CMD`下需要执行`chcp 65001`使其支持UTF-8，具体操作方式可以参考[Jekyll 本地调试之若干问题](http://chxt6896.github.io/blog/2012/02/13/blog-jekyll-native.html)。  
  * 首页的摘要使用了[为Jekyll增加不完美的分页和文章摘要](http://kingauthur.info/2013/01/20/the-paginator-and-excerpt-in-jekyll/)这篇文章中介绍的方法，在此感谢博主无私的分享。

***
最后的最后，老惯例~感谢众多大牛以及博主们做的贡献，感谢CCAV, 感谢MTV 。。。。。