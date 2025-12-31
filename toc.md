---
title: Site-wide Table of Contents
toc: false
---

<ul>
{% for page in site.pages %}
    {% if page.toc_data %}
  	<li>
        <a href="{{ page.url }}">{{ page.title }}</a>
    	<ul>
   	 {% for item in page.toc_data %}
      		<li><a href="{{ item.link }}">{{ item.title }}</a></li>
    	 {% endfor %}
    	</ul>
  	</li>
    {% endif %}
{% endfor %}
</ul>
