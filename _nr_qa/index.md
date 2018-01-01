---
title: 'Node-RED Questions and Answers'
description: 'Node-RED is a flow-based (visual) programming tool. These pages have some information that may be currently missing from the documentation.'
comments: false
---

<ul>
{% for item in site.nr_qa %}
  <li>
    <a href="{{ item.url }}">{{ item.title | replace:'_',' ' }}</a> ({{ item.date | default: site.time | date: "%Y-%m-%d %H:%M" }})
    <p>{% if item.description %}
        {{ item.description }}
    {% else %}
        {{ item.excerpt | strip_html }}
    {% endif %}</p>
  </li>
{% endfor %}
</ul>
