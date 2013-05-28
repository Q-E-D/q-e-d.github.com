---
layout: post
title: "seajs实现前端模块化之Hello World姐妹篇之部署看我的"
description: "seajs合并压缩, 部署"
category: JavaScript
tags: [Sea.Js, 前端模块化]
---
{% include JB/setup %}

本文作为[seajs实现前端模块化之Hello World](/JavaScript/seajs-hello-world/)的姐妹篇，补充一下上文中缺失的压缩、合并等部分，方便上线部署。本文涉及的东西教新，博主表示正处于初级发展阶段，所以有些地方可能叙述不准或者过简，遇到这种问题可以自行查阅资料或者留言咨询或者点击关闭按钮→ →

***

### 背景

  在前文中我们完成了变色龙的部分，功能全部正常，但是与一个问题，就是性能。打开firebug果断额围观下:  
  ![合并压缩前加载情况](/assets/images/2013-05/seajs_hello_world_load_before.png)  
  果断需要压缩合并。由于使用了seajs，所以还不能用普通的压缩合并方式，走起，边看边说。

###安装环境

先来热身运动，[Node.Js](http://nodejs.org/)，具体的介绍可以参考官网，你可以把它当成是JRE之类的东东，只不过使用的是*JavaScript*而不是*Java*，而且不仅款平台，还前后端通吃--!**非阻塞IO**、**事件驱动**使得其直接进入了高性能开发的候选人，现在已近被部分公司采用，效果惊人，同学们可以多研究研究。不过现在不急、先下载`Node.Js`安装。基本一路*next*即可。。。

接下来是[Grunt](http://gruntjs.com/)，可以把它看成是`Ant`，一个总司令，可以知道下面的插件帮你解决各种需要手动重复是事情。前端自动化的利器、构建高效工作流的工具箱。这里你也不先记着深入研究，先来安装一发。在`Node.Js`安装好的前提下(**path**已经配置)，在命令行下运行:

	npm install -g grunt-cli

安装一个管理`grunt`的东东，大概的功能就是让你可以在电脑上同时安装不同版本。下面就开始搞起。

###插件安装

 在这篇文章里面我们需要进行的工作为压缩、合并、总过需要使用下面几个插件：  

  * grunt				  Grunt里面指挥干活的,刚才那个`grunt-cli`是用于管理不同版本的
  * grunt-cmd-transport   提取ID的，不然Sea.Js里面的require不好处理滴。
  * grunt-cmd-concat      合并文件
  * grunt-contrib-uglify  压缩文件
  * grunt-contrib-clean   清理临时目录

  `Node.Js`里面安装插件很是简单，不过记得将其加进依赖。首先进入`hello-seajs\assets\app`目录新建一个`package.json`文件，用于描述项目信息、依赖等，内容为： 

	{
	  "name": "hello",
	  "version": "0.1.0",
	  "devDependencies": {
	    "grunt": "~0.4.1",
	    "grunt-cmd-transport": "~0.1.1",
	    "grunt-cmd-concat": "~0.1.0",
	    "grunt-contrib-uglify": "~0.2.0",
	    "grunt-contrib-clean": "~0.4.0"
	  }
	}

  具体每个字段的意思这里就不一一说明，同学们自行查阅。文件新建完成后，命令行切换到当前目录运行一下命令完成Grunt的安装：  

	npm install

  到这，插件安装完成~~当然，安装插件可以使用`--save-dev`那个东东，安装的同时可以加入依赖。

### 插件配置

  在`package.json`的目录下再新建一个文件`Gruntfile.js`，在里面我们进行配置工作。我们的步骤为: 提取ID-->合并文件-->压缩-->清理，下面附上配置文件的内容：  

	module.exports = function(grunt) {

	  grunt.initConfig({
	    transport: {
	      options: {
	        format: 'app/dist/{{filename}}'
	      },
	      chameleon: {
	        files: {
	          '.build': ['main.js', 'names.js', 'colorUtils.js', 'chameleon.js']
	        }
	      }
	    },
	    concat: {
	      chameleon: {
	        options: {
	          relative: true
	        },
	        files: {
	          'dist/main.js': ['.build/main.js', '.build/names.js', '.build/colorUtils.js', '.build/chameleon.js']
	        }
	      }
	    },
	    uglify: {
	      main: {
	        files: {
	          'dist/main.min.js': ['dist/main.js']
	        }
	      }
	    },
	    clean: {
	      build: ['.build']
	    }
	  });
	
	
	
	  grunt.loadNpmTasks('grunt-cmd-transport');
	  grunt.loadNpmTasks('grunt-cmd-concat');
	  grunt.loadNpmTasks('grunt-contrib-clean');
	  grunt.loadNpmTasks('grunt-contrib-uglify');
	
	  grunt.registerTask('build', ['transport', 'concat', 'uglify', 'clean']);
	};

  前面大部分是插件的配置，功能少故比较简单，使用了很多默认的信息，如果需要深入了的话最好查阅插件的官网以便获得最新动态。在文件的后半部分，我们使用了

	grunt.loadNpmTasks('xxx')
  加载了插件，最后面这是注册一个默认的job名叫`build`，job的具体内容可以自行配置，出现的顺序影响执行顺序。

### 运行

  配置完成了，再当前目录下(安装了grunt的目录)运行一下命令:

	grunt build

  ok，任务就完成了。打开`app/dist`目录，我们已经看到*main.min.js*了。现在切换我们也的引用，在此查看，结果就是NICE了。  
  ![合并压缩后加载情况](/assets/images/2013-05/seajs_hello_world_load_after.png)


### 注意事项

  * `package.json`以及`Gruntfile.js`文件的位置需要放在你要出来的js的目录下，不然在进行ID提取时将产生`\`与`/`混用的效果--!需要等待新版本，可以参看一下[这里](https://github.com/spmjs/grunt-cmd-transport/issues/6)。

  * 配置ID提取时，需要加入:  

    	options: {
        	format: 'app/dist/{{filename}}'
      	}
    否则提取的ID里面带有*[object Object]*。同时，其值要与路径信息匹配。在我们的项目中，我们的js文件都放在了app目录下，该目录下我们有临时使用的文件夹`.build`，同时`dist`目录还有最后压缩合并后的文件。因为最终的文件位置为`app/dist/main.min.js`，所以，页面引用是就是使用此路径，同时，这个文件的模块化ID也必须为这个，所以上文的配置信息为`app/dist`;

	