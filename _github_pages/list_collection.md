---
title: How to generate a list of collection documents
description: >
  The following code automatically generates a list of collection documents.

  It assumes that the collection uses `output: true` and optionally includes a collection title
  and description defined in the `_config.yml` configuration file.
comments: true
date: 2018-01-05 17:00:00
---

### collection_doc_lister.html include file

Create this as a file in your _includes folder

{% raw %}
```html
{% assign collection = include.collection %}
{% assign colName = include.collection | replace: 'site.',''  %}
{% assign col = site.collections | where: 'label',colName | first %}
{% assign colTitle = col.title | default: colName | replace: '_',' ' | replace: '-', ' ' | capitalize %}

<h2><a href="{{ colName | prefix: '/' | append: '/' }}">{{ colTitle }}</a></h2>

{% if col.description %}<p>{{ col.description }}</p>{% endif %}

<div>
  <ul>
  {% for item in collection | sort: "title" %}
    <li>
      {% assign mytitle = item.title | default: item.url %}
      <a href="{{ item.url }}">{{ mytitle | replace:'_',' ' | replace:'-',' ' }}</a> {% if item.date %}({{ item.date | date: "%Y-%m-%d %H:%M" }}){% endif %}
      <p>{% if item.description %}
          {{ item.description }}
      {% else %}
          {{ item.excerpt | strip_html }}
      {% endif %}</p>
    </li>
  {% endfor %}
  </ul>
</div>
```
{% endraw %}

### Using the include file

To use, place the following code wherever you want a list of the output files - such as the index.md for the collection folder.

{% raw %}
```markdown
{% include collection_doc_lister.html collection=site.github_pages %}
```
{% endraw %}

Note the reference to the `site.[collection_name]` variable which is passed to the include file.
