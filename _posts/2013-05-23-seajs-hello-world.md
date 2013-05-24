---
layout: post
title: "seajs实现前端模块化之Hello World"
description: "seajs hello world"
category: JavaScript
tags: []
---
{% include JB/setup %}

JavaScript，一种神奇的语言。百分之九十以上的程序员都会在其简历上写有  
> 熟练掌握JavaScript

国内这个环境下，绝大多数公司都没有专职的*前端工程师*，页面的Javascript效果都是后台程序员或者美工兼职完成的。而这些人中的大部分都只是在遇到需要的时候问下*度娘*或者*google君*，使用下JQuery之类的类库、绑定事件、写点简单的动画，其余时间仅仅使用插件即可。时间久了，很多人就自以为JavaScript不过尔尔，然后就发生上述情况了。

其实，JavaScript的一生也是命运多舛。开始出生的时候就早产(Brendan Eich表示仅使用了10天完成了语言的演示原型，书写了编译器和运行环境。。具体内情自行搜索)，而后被applet和flash死死压着，直到后来Ajax的兴起，Google各种赤裸裸的技术炫耀(Gmail， Reader)，大家发现原来Javascript可以这么强大。再到后面coffeescript，以及Node.js等等，基本以及从前端页面一直蔓延到服务端。只能说、金子迟早会发光的，即使包含着糟粕。

有志之士可以阅读以下[JavaScript语言精粹](http://book.douban.com/subject/11874748/)快速了解，需要深入研究的围观[JavaScript权威指南](http://book.douban.com/subject/10549733/)。

好吧，广告完了我们开始正题。。。

***

###背景介绍
SeaJs的介绍[官网](http://seajs.org/docs/)上面写得很详细，大家可以去强势围观。虾米是*前端模块化*，[前端模块化开发那点历史](https://github.com/seajs/seajs/issues/588)这边文章里面有介绍。为啥要模块化？好吧，猛击这里[前端模块化开发的价值](https://github.com/seajs/seajs/issues/547)。为什么。。。打住打住，今天主要内容是Hello World！有疑问执行google或者留言~~

###Hello World说明

虽然是Hello World，但是我们要严肃对待。下面说一下它的主要功能：  

* 页面载入完毕后弹出欢迎信息。
* 页面上有很多色块，起始是黑色，每个色块随机使用一个预先定义好的名字。每个点击后会随机变色，类似变色龙。
* 点击*还原*按钮后，色块恢复起始状态。

大致的功能就像下图：  
![seajs之hello world变色龙成品](/assets/images/2013-05/seajs_hello_world_1.png) 
 
![seajs之hello world变色龙成品](/assets/images/2013-05/seajs_hello_world_2.png)

很简单的几个小功能，下面我们开始来干货。


###规划

首先我们规划下，大概的结构如下图：  
![seajs之hello world变色龙项目结构图](/assets/images/2013-05/seajs_hello_world_dirs.png)  
其中，`main.js`作为主要入口，`chameleon.js`就是变色龙君，`colorUtils.js`负责随机色。

###代码

首先，我们来写`colorUtils.js`这个模块。此模块不需要外部的资源，输出也仅有一个，果断采取简要写法:    

	define({
		getColor : function(){
			var num = Math.ceil(Math.random() * 65536*16*16);
			return "#" + num.toString(16);
		}
	});

`define();`这是SeaJs使用的关键字+基本模块结构，用于定义一个**模块**，里面可以看成是一个*JSON*格式的对象。这种写法简单明了，但是需要注意到对象的所有属性向外暴露。这里我们定义了一个属性叫`getColor`， 它是一个不需要参数的函数，返回一个代表颜色的字符串。

接下来我们写`chameleon.js`， 这个模块是核心模块，处理所有事件。我们需要绑定事件，然后处理CSS，果断使用jquery。在这里我们需要注意，当我们使用SeaJs引入模块时，每个模块的格式是有要求的，必须遵循一个的规范(否则人家怎么失败咧。。。)。但是现在现成的JS库那么多，尤其是茫茫多的插件库，难道我们要舍弃？肯定不能呀！果断使用插件`shim`,它能够十分方便的将没有遵循规范的库整合进来^_^。在`config.js`文件里面配置一下：  

	seajs.config({
	  plugins: ['shim', 'text'],
	
	  alias: {
	    'jquery': {
	      src: 'lib/jquery-2.0.0.min.js',
	      exports: 'jQuery'
	    }
	  }
	});

格式大概就是上面的样子，`plugins`属性引用了两个插件，另外一个`text`插件用于从外部文件中加载JSON格式的数据，后文具体介绍。`alias`属性则是设置别名，免得我们每次引入模块的时候都要输N长的路径。配置信息还有很多，具体精确的解释清参考官网。

JQuery有了，下面我们开始干活。打开`chameleon.js`，写代码的走起。

	define(function(require, exports, module){
		
		var $ = require("jquery");
		var cu = require("app/colorUtils.js");
		var names = require("app/names");		

	});

这个才是模块的比较标准的格式，大家默认使用即可，需要深入了解的自行查询。入口处，我们使用了`require`引入外部模块，引入后，我们就可以像老式的那样自由调用我们定义在模块里面的函数了。不过先不急，我们先定义*变色龙*君。

	function Chameleon(container, child, btn){
		this.container = $(container);
		this.child = child;
		this.btn = $(btn);
	}

普通的类写法，没啥特殊的，后文肯定需要补充，这里先将外部接口放出供其他模块使用：  

	module.exports = Chameleon;

到这，模块的定义完成。引入、处理、导出，很简单~接下来就是完善我们的变色龙君，毕竟上面我们杀都没干。  

	Chameleon.prototype.setup = function(){
		this._init()
			._bindEvent();

		return this;
	};

	Chameleon.prototype._init = function(){
		var arrs = names.slice(0, 5);
		
		$(this.child).each(function(){
			var idx = Math.floor(Math.random() * arrs.length);
			$(this).text(arrs[idx]);
			arrs.splice(idx, 1);
		});

		return this;
	};

	Chameleon.prototype._bindEvent = function(){
		var child = this.child;
		var that = this;

		$(this.container).on("click", this.child, function(){
			$(this).css("background-color", cu.getColor());
		});

		$(this.btn).click(function(){
			$(child).css("background-color", "black");
			that._init();
		});

		return this;
	};

代码的功能和逻辑很简单，和普通的JS的写法一样，这里就不在细说，如果哥们你看不懂。。。推荐文章起始处的书籍。。。

功能实现了，现在开始搞定入口。打开`main.js`，代码非常精简：  

	define(function(require){


		var Chameleon = require("app/chameleon");
	
		var cl = new Chameleon("#container", ".colorBlock", "#resetBtn");
		cl.setup();
	
		alert("欢迎进入SeaJs的世界");
	});

这里引入了我们之前定义的`变色龙`君，调用，没了，后面打印了欢迎信息。是不是很简单呀，简单就对了，这就是模块封装之美，不过下面还有更美的。模块全部搞定了，最后需要解决的问题就是这么在页面上执行上面的入口代码，一句话，没错！就是一句话。

	<script type="text/javascript" src = "assets/seajs/sea.js" id = "seajsnode" data-config = "app/config.js" data-main = "app/main.js"></script>

以普通引入*JavaScript*的方式引入`sea.js`，其中`id`字段推荐如上文所示，不要变动，可以加快模块初始化的速度。后面的`data-config`指向我们前文中的配置文件， `data-main`则指向入口文件。就是这么一句话，OK了，其他的没有了，原来，页面可以这么美丽→ →

现在，打开你的浏览器，围观吧，少年！！！！！！！！

###最后的最后

关于压缩合并的部分，由于又是设计Grunt,Node.js等众多新东西，留待下次进行再行细说。

由于要本人能力有限，可能文章中有诸多表述不清或者表述错误的地方，欢迎留言咨询讨论。当然，最好的学习方式就是自己动手试一试，多看看官网DOC，多使用Google，这个是学习任何新技术都一样滴~好了，废话不多说，耍起吧，少年！