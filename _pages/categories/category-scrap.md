---
title: "어쩌면 이것은 희망과 도전에 대한 이야기입니다."
layout: archive
permalink: categories/scrap
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.scrap %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}