---
title: AI-Driven HPC application tutorials in Bioinformatics and Life Sciences 
author: CSC Training
---

{% assign items = site.hands-on |  sort: "title" %}

## 1. Genomics applications
### Slides: Introduction to deepvariant method

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'genomics' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2. Docking applications
### Slides:  Introduction to docking methods

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'docking' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}
