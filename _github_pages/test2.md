---
title: A Test Page
description: Testing collections and page variables - this page is in a collection and is a markdown file
comments: false
testFm: testing front matter variables
date: 2018-01-02 17:11:00
---

<h1>This is a {% if page.collection %}COLLECTION DOCUMENT{% else %}JEKYLL PAGE{% endif %}</h1>

<p>Here are some page variables:</p>
<dl>
    <dd>page.url</dd>
    <dt>{{ page.url }}</dt>

    <dd>page.layout</dd>
    <dt>{{ page.layout }}</dt>

    <dd>page.path</dd>
    <dt>{{ page.path }}</dt>

    <dd>page.date</dd>
    <dt>{{ page.date }}</dt>

    <dd>page.testFm</dd>
    <dt>{{ page.testFm }}</dt>

    <dd>page.comments</dd>
    <dt>{{ page.testFm }}</dt>

    <dd>page.title</dd>
    <dt>{{ page.title }}</dt>

    <dd>page.description</dd>
    <dt>{{ page.description }}</dt>

    <dd>page.id (collection documents only)</dd>
    <dt>{{ page. }}</dt>

    <dd>page.collection (collection documents only)</dd>
    <dt>{{ page. }}</dt>

    <dd>page.dir (actual Jekyll Pages only)</dd>
    <dt>{{ page.dir }}</dt>

    <dd>page.name (actual Jekyll Pages only)</dd>
    <dt>{{ page.name }}</dt>
</dl>

<script>
    (function() {
        // Dump the page object to a JS variable - note we have to strip or escape the html
        var jk_page = {{ page | jsonify | strip_html }};
        console.log('--PAGE (jsonify)--', jk_page)
    })();
</script>

