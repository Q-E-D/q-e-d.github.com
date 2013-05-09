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
		<a href="{{ BASE_PATH }}{{ post.url }}" class = "postTitle">{{ post.title }}</a>		
		<div class = "postContent">
			{{ post.content | truncate: 180 }}
		</div>
		<div>
			<a href = "{{post.url}}" class = "readMore">阅读全文..</a>
			<i class="icon-tag"></i>
			{% for tag in post.tags %}
				<span>{{tag}}</span>
			{% endfor %}
		</div>
	</li>

	{% endfor %}
</ul>


