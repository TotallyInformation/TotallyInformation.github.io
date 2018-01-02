---
title: VSCode Hints and Tips
description: >
    Visual Studio Code is an Integrated Development Environment (IDE) or code editor that is open source and actively developed by Microsoft.
    These articles contain ideas, hints, tips and knowledge about it and how to use it.
comments: true
---

{% include page_lister.html dir="/vscode/" %}

<hr>
<ul>
{% assign mypages = site.pages | where:"dir", "/vscode/" | where_exp:"item", "item.name != page.name" %}
{% for item in mypages | sort: "title" %}
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

<script>
    (function() {
        var mypage = {{ page | jsonify | strip_html }};
        console.log('--PAGE (jsonify)--', mypage)
        var mypages = {{ mypages | jsonify | strip_html }};
        console.log('--PAGES (jsonify)--', mypages)
        var sitepages = {{ site.pages | jsonify | strip_html }};
        console.log('--SITEPAGES (jsonify)--', sitepages)
    })();
</script>
