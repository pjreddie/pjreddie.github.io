---
layout: projects
title: "Projects"
permalink: /projects/
---

{% assign pages = site.projects | sort: "order" %}
{% for page in pages %}
{% include preview.html page=page %}
{% endfor %}
