---
title: "파이썬 빠르게 복습"
layout: archive
permalink: categories/introduction-to-python
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories['Introduction to Python'] %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}
