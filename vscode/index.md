---
title: VSCode Hints and Tips
description: >
    Visual Studio Code is an Integrated Development Environment (IDE) or code editor that is open source and actively developed by Microsoft.
    These articles contain ideas, hints, tips and knowledge about it and how to use it.
comments: true
---

<ul>
{% for item in site.pages where:"dir", "/vscode/" | sort: "title" %}
  <li>
    <a href="{{ item.url }}">{{ item.title | replace:'_',' ' }}</a>
    <p>{% if item.description %}
        {{ item.description }}
    {% else %}
        {{ item.excerpt | strip_html }}
    {% endif %}</p>
  </li>
{% endfor %}
</ul>
