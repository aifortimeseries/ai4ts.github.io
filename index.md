   ---
   layout: page
   title: Home
   ---

## Table of Contents

{% for section_name in site.section_order %}
- [{{ site.section_metadata[section_name].title }}](#{{ section_name }})
{% endfor %}

---

{% for section_name in site.section_order %}
  {% assign section_meta = site.section_metadata[section_name] %}
  {% assign items = site[section_name] | sort: 'order' %}
  
<h2 id="{{ section_name }}">{{ section_meta.title }}</h2>

  {% if items.size > 0 %}
    {% for item in items %}

### {{ item.title }}

{% if item.author %}*Author: {{ item.author }}*{% endif %}
{% if item.date %}*Date: {{ item.date | date: site.minima.date_format }}*{% endif %}
{% if item.status %}*Status: {{ item.status }}*{% endif %}

{{ item.content }}

[Edit on GitHub](https://github.com/aifortimeseries/ai4ts.github.io/edit/main/{{ item.path }}){:target="_blank"}

---

    {% endfor %}
  {% else %}
*This section is currently empty. [Add first item](https://github.com/aifortimeseries/ai4ts.github.io/new/main/{{ section_name }}){:target="_blank"}*
  {% endif %}
  
{% endfor %}

---

*Maintained by IEEE P3579 and W3C WAITS CG*
