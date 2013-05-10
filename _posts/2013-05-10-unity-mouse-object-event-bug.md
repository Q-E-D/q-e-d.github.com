---
layout: post
title: "unity mouse object event bug"
description: "unity3d绑定鼠标hover物体事件bug"
category: unity
tags: [unity, c#, bug]
---
{% include JB/setup %}

风萧萧兮易水寒，楼主搞起unity，兼用c#，由于对两位同志都不是很了解，今天遇到了一个极其古怪的问题。之后各种google强势围观，奈何依然未果。最后在围观一个**死**胖子(那个胖子真的很胖~)的代码的时候， 暮然回首，果断KO。记录于此，待有缘人。

***

事情开始于需要用unity做一个普通游戏中常见的功能，鼠标悬浮到物体上时，高亮突出显示；移出后恢复原状。刚才的*死胖子*君提供[unity3d用鼠标拖动物体的一段代码](http://wtuu.blog.163.com/blog/static/16981550201088115430459/)文章，虽然是js的，但是思路逻辑完整，果断移植`c#`进行`微创新`。

	private Color mouseOverColor = Color.blue;
	private Color originalColor;
	
	void Start () {
		originalColor = renderer.material.color;
	}
	
	void OnMouseEnter (){
		renderer.material.color = mouseOverColor;
	}
	
	void OnMouseExit () {
	    renderer.material.color = originalColor;
	}

果断跑去运行，结果一大波bug迎面而来:

> The name `originalColor' does not exist in the current context

分别指向`start`函数和`OnMouseExit`函数。当时我就在风中凌乱了，虽然我自认为`c#`学的比较差，但是也不能这样玩我的说。。。那个赤裸裸的私有变量啊，怎么会没法访问的说。期间google了一下，大部分都是指向类型不匹配，于是果断测试:  

	void Start () {
		Color originalColor = renderer.material.color;
		Debug.Log(originalColor);
	}

unity表示毫无压力，于是我只好默默的折腾，比方说给originalColor赋初值:  

	private Color originalColor;

亦或改变其作用域:   
	
	public Color originalColor;

unity依然表示*我还是太年轻*。接下来只好开大招，果断呼唤*死胖子*君(是的，必须加个**死**字以表示强调→ →)。*死胖子*果断提供了代码:  
![胖子的代码](/assets/images/2013-05/unity-mouse-object-event-panzi.jpg)  
我一看，区别太多了，果断懒得去改啊，默默的点了叉叉。。。  
突然，一股电流穿过我的身体(强烈表示不是被雷劈了！)， 我仿佛那个啥啥啥附体，功力大增，直接买入那个啥的境界了。好吧，我承认我NC了，小说看多了哈。。。。总而言之，加上一个`this`啊，亲~  

	void Start () {
		this.originalColor = renderer.material.color;
	}

结果，unity表示还是我比较凶残，果断放行了。此时此刻， 我好想仰天长啸，然后再小人得志的队unity同学说：　　
> 少年， 你还是太年轻了哈。  

如果你以为这就是ending，那就太没意思了，故事才刚刚开始。。。。

***

我琢磨着这么诡异的东西都遇到了，总得记录下来，万一哪天哪个倒霉的同学遇到了说不定能用上。于是二话不说开始码字。但是当我码字码到文章开始部分，要演示错误代码的时候，我去把`this`删掉，然后运行准备截图，结果，结果竟然没有任何错误提示的通过了！！！！！！！！这是要闹哪样啊，少年。。。至此，我表示我彻底的虚了，unity君，我还是太年轻。。。

文中出现的所有情节均属事实，这个可以咨询**死胖子**同学，最后我们俩表示还是太年轻，只好记录于此，待高玩或大能解决， 阿门。