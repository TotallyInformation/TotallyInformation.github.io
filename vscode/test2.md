---
title: A Test Page (MarkDown)
description: Testing collections and page variables - this page is in a collection and is a markdown file
comments: false
testFm: testing front matter variables
date: 2018-01-02 17:11:00
---

<h1>This is a {% if page.collection %}COLLECTION DOCUMENT{% else %}JEKYLL PAGE{% endif %}</h1>

<p>Here are some page variables:</p>
<dl>
    <dd>page.url</dd>
    <dt>{{ page.url | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.layout</dd>
    <dt>{{ page.layout | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.path</dd>
    <dt>{{ page.path | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.date</dd>
    <dt>{{ page.date | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.testFm</dd>
    <dt>{{ page.testFm | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.comments</dd>
    <dt>{{ page.testFm | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.title</dd>
    <dt>{{ page.title | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.description</dd>
    <dt>{{ page.description | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.id (collection documents and Posts only)</dd>
    <dt>{{ page.id | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.collection (collection documents only)</dd>
    <dt>{{ page.collection | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.dir (actual Jekyll Pages only)</dd>
    <dt>{{ page.dir | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.name (actual Jekyll Pages only)</dd>
    <dt>{{ page.name | default:"--NOT AVAILABLE--" }}</dt>
</dl>

<script>
    (function() {
        // Dump the page object to a JS variable - note we have to strip or escape the html
        var jk_page = {{ page | jsonify | strip_html }};
        console.log('--PAGE (jsonify)--', jk_page)
    })();
</script>

