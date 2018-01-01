---
title: Home
description: 'This site is used to publish development-related reference information curated by Totally Information.'
someData: Some Text for someData
comments: false
---

## Node-RED Questions and Answers

Node-RED is a flow-based (visual) programming tool. These pages have some information that may be currently missing from the documentation.

<ul>
{% for item in site.nr_qa %}
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

## GitHub Pages and Jekyll/Liquid

Hints and tips on using Jekyll for publishing to GitHub Pages.

<ul>
{% for item in site.github_pages %}
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

## Totally Information's Public Code Repositories

<table>
    {% tablerow repository in site.github.public_repositories cols:1 %}
        <a hre="{{ repository.html_url }}">{{ repository.name }}</a>
    {% endtablerow %}
</table>

<script>
    (function() {
        // Dump the page object to a JS variable - note we have to strip or escape the html
        var jk_page = {{ page | jsonify | strip_html }};
        var someData = '{{ page.someData }}'
        var layout = '{{ layout }}'
        console.log('--PAGE (jsonify)--', jk_page)
        console.log('someData', someData)
    })();
</script>
