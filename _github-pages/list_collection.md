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
{% assign colName = include.collection %}
{% assign col = site.collections | where: 'label',colName | first %}

<div class="collection-list">
{% if col %}

  {% assign collection = site[col.label] %}
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

{% else %}

  <p>There is no site collection called <code>{{ colName }}</code>.</p>

{% endif %}
</div>
```
{% endraw %}

### Using the include file

To use, place the following code wherever you want a list of the output files - such as the index.md for the collection folder.

{% raw %}
```markdown
{% include collection_doc_lister.html collection='github_pages' %}
```
{% endraw %}

Note the reference to the collection name which is passed to the include file.
