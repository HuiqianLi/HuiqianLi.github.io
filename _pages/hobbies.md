---
layout: archive
title: "Hobbies"
permalink: /hobbies/
author_profile: true
---

{% include base_path %}

{% for post in site.hobbies %}
  {% capture post.title %}'None'{% endcapture %}
  {% include archive-single.html %}
{% endfor %}

