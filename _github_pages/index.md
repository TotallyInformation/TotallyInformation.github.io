---
title: 'GitHub Pages and Jekyll/Liquid'
description: 'Hints and tips on using Jekyll for publishing to GitHub Pages.'
comments: false
---

<ul>
{% for item in site.github_pages %}
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


{% include collection_doc_lister.html collection=site.github_pages %}

<script>
    (function() {
        var collection = {{ site.github_pages | jsonify | strip_html }};
        console.log('--COLLECTION (jsonify)--', collection)
    })();
</script>
