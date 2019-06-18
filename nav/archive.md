---
layout: page
title: archive
permalink: /archive/
header-img: "img/head.jpg"
---

### Blogs
<hr>

{% for post in site.posts %}
<div class="post-preview">
    <font color="black">[{{ post.date | date: "%B %-d, %Y" }}]  </font> 
     <a target="_blank" href="{{ post.url | prepend: site.baseurl }}"> {{ post.title }}  </a> 
</div>
<hr>
{% endfor %}

