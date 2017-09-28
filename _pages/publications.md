---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---


My articles on <u><a href="https://scholar.google.com/citations?user=rWOdLRYAAAAJ">Google Scholar</a>.</u>


{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
