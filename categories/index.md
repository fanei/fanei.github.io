---
layout: archive
permalink: /categories/
title: "Latest Posts in *categories*"
excerpt: "I make living from IT"
---

<div class="tiles">
{% for post in site.posts %}
	{% if post.categories contains 'categories' %}
		{% include post-list.html %}
	{% endif %}
{% endfor %}
</div><!-- /.tiles -->
