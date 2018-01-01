---
title: Home
description: 'This site is used to publish development-related reference information curated by Totally Information.'
someData: Some Text for someData
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
  <p>Do we get all the page variables for this page which uses the "Default" template? Nope.</p>
  &#10004; 'page.url': --{{ page.url }}--<br>
  &#10004; 'page.layout': --{{ page.layout }}-- : Default for this collection is "page" but set to "home" in front matter - should show "home"<br>
  &#10004; 'page.type': --{{ page.type }}--<br>
   'page.date': --{{ page.date }}-- : Should be auto-set according to docs but apparently not.<br>
   'page.path': --{{ page.path }}--<br>

  <p>Custom variables set in _config.yml or front matter? <b>Only available to actual Pages and NOT TO COLLECTION DOCUMENTS!</b></p>
  &times;  'page.title': --{{ page.title }}-- : This is set in this page's front matter, it should be available! <br>
  &times;  'page.description': --{{ page.description }}-- : This is set in this page's front matter, it should be available! <br>
  &times;  'page.comments': --{{ page.comments }}-- : Is set in this page as false but is, instead showing true which is the default<br>
  &times;  'page.borderColor2': --{{ page.borderColor2 }}--: Is set in this page's front matter, it should be available!<br>

  <p>Local assigned variables? Yes.</p>
  &#10004; 'borderColor': --{{ borderColor }}-- : Is set in an assignment tag so is available.<br>

  <p>Only available in collection documents like this</p>
   'page.id': --{{ page.id }}-- : Should be set for documents in a collection or a post so should be available here (collection document).<br>
   'page.collection': --{{ page.collection }}-- : Should be set for documents in a collection or a post so should be available here (collection document).<br>

  <p>Only available in actual pages (not a collection document like this)</p>
  &#10004; 'page.dir': --{{ page.dir }}--<br>
  &#10004; 'page.name': --{{ page.name }}--<br>
-->

<script>
    (function() {
        // Dump the page object to a JS variable - note we have to strip or escape the html
        var jk_page = {{ page | jsonify | strip_html }};
        var someData = {{ page.someData }}
        var layout = {{ layout }}
        console.log('--PAGE (jsonify)--', jk_page)
        console.log('someData', someData)
    })();
</script>
