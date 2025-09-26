---
title: Containers in Supercomputing Environment
author: CSC Training
---

{% assign items = site.hands-on |  sort: "title" %}

## 1. Introduction to ML/AI
### Slides: [Introduction to CSC HPC environments](https://a3s.fi/biocontainers2023/CSC_HPC_Systems.html)
### Slides: [Introduction to Containers](https://a3s.fi/biocontainers2023/Introduction_to_containers.html)

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'bioapplications' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}
