---
title: Introduction to python 책
layout: archive
permalink: categories/python-입문
author_profile: true
sidebar_main: true
---


{% assign posts = site.categories.['Python 입문'] %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}