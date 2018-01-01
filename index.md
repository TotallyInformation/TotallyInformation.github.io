---
description: 'This site is used to publish development-related reference information curated by Totally Information.'
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

<!--
<div style="border:1px solid silver">
  <p>Do we get all the page variables for this page which uses the "Default" template? Nope.</p>
  &times; {{ page.author | default: 'should be `author`'}} <br>
  &times; {{ page.handle | default: 'should be `handle`' }} <br>
  &#10004; {{ page.id | default: 'should be `id`' }} <br>
  &times; {{ page.published_at | default: 'should be `published_at`' }} <br>
  &times; {{ page.template_suffix | default: 'should be `template_suffix`' }} <br>
  &times; {{ page.title | default: 'should be `title`' }} <br>
  &#10004; {{ page.url | default: 'should be `url`' }}
  &times; 'page.borderColor2': --{{ page.borderColor2 }}--<br>
  &#10004; 'page.comments': --{{ page.comments }}--<br>
  &#10004; 'page.layout': --{{ page.layout }}--<br>
  &#10004; 'page.type': --{{ page.type }}--<br>
  <p>Local assigned variables? Yes.</p>
  &#10004; 'borderColor': --{{ borderColor }}--<br>
</div>
-->
