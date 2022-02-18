---
title: "베끼다"
layout: archive
permalink: categories/scribe
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.scribe %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}