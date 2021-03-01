---
layout: post
title: "カテゴリ別投稿一覧"
date: 2021-02-10 18:30
description: カテゴリ別投稿一覧
author: JJ1BBY
---
{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
