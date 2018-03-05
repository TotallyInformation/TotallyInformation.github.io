---
title: How to generate a list of pages in a given folder
description: >
  The following code automatically generates a list of pages that exist in a given folder.
comments: true
date: 2018-01-05 17:00:00
---

### page_lister.html include file

Create this as a file in your _includes folder

{% raw %}
```html
{% assign dir = include.dir | default: page.dir %}
<div class="page-list">
{% assign mypages = site.pages | where:"dir", dir | where_exp:"item", "item.name != page.name" %}
{% if mypages.length > 0 %}
  <ul>
  {% for item in mypages | sort: "title" %}
    {% assign mytitle = item.title | default: item.url %}
    <li>
      <a href="{{ item.url }}">{{ mytitle | replace:'_',' ' | replace:'-',' ' }}</a>
      <p>{% if item.description %}
          {{ item.description }}
      {% else %}
          {{ item.excerpt | strip_html }}
      {% endif %}</p>
    </li>
  {% endfor %}
  </ul>
{% else %}
  <p>No pages found in folder <code>{{ dir }}</code>.</p>
{% endif %}
</div>
```
{% endraw %}

### Using the include file

To use, place the following code wherever you want a list of the output files - such as the index.md for the collection folder.

{% raw %}
```markdown
{% include page_lister.html dir="/vscode/" %}
```
{% endraw %}

Note the reference to the folder directory name (with leading/trailing slashes as per the `.dir` attribute) which is passed to the include file.
