---
layout: page
title: DevOps
permalink: /blog/categories/DevOps
---
 
<h5> Posts by Category : {{ page.title }} </h5>

<div class="card">
<!-- Change the category here -->
{% for post in site.categories.DevOps %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>