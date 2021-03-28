---
layout: default
title: Blog
---

# 最新文章

<!-- 链接到全部博客(for循环遍历) -->
{% for post in site.posts %}
    {{ post.date | date_to_string }} [{{ post.title }}]({{ post.url }})
    <!-- 默认内容的第一段 -->
    {{ post.excerpt }}
{% endfor %}
