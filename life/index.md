---
layout: archive
permalink: /life/
title: LIFE
excerpt: "living my life"
---

<div class="tiles">
{% for post in site.posts %}
	{% if post.categories contains 'life' %}
		{% include post-list.html %}
	{% endif %}
{% endfor %}
</div><!-- /.tiles -->
