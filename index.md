---
layout: page
title: 低调的酱油党
tagline: 默默coding的码农
---
{% include JB/setup %}
{{ tag_cloud }}

<ul class="posts">
  {% for post in site.posts %}
    <li>
		<a href="{{ BASE_PATH }}{{ post.url }}" class = "postTitle" target = "_blank">{{ post.title }}</a>		
		<div class = "postContent">
			{{ post.content | truncate: 184 }}
		</div>
		<div>
			<a href = "{{post.url}}" class = "readMore" target = "_blank">阅读全文..</a>
			<i class="icon-tag"></i>
			{% for tag in post.tags %}
				<a class = "postTag" href = "/tags.html#{{tag}}-ref">{{tag}}</a>
			{% endfor %}
		</div>
	</li>

	{% endfor %}
</ul>


