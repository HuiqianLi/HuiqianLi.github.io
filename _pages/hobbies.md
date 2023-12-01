---
layout: archive
title: "Hobbies"
permalink: /hobbies/
author_profile: true
---

{% include base_path %}

{% for post in site.hobbies reversed %}
  {% include archive-single.html %}
{% endfor %}

