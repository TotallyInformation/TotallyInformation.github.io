---
title: A Test Jekyll/GitHub Pages Page (MarkDown)
description: Testing collections and page variables - this page is an actual Jekyll Page and is in a folder, it is a markdown file
comments: test
testFm: testing front matter variables
date: 2018-01-02 17:11:00
---

<h1>This is a {% if page.collection %}COLLECTION DOCUMENT{% else %}JEKYLL PAGE{% endif %}</h1>

<p>Here are some page variables:</p>
<dl>
    <dd>page.collection (collection documents only)</dd>
    <dt>{{ page.collection | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.comments (custom, set in <code>_config.yml</code> defaults but also in frontmatter)</dd>
    <dt>{{ page.comments | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.content (actual Jekyll Pages only?)</dd>
    <dt>{{ page.content | default:"--NOT AVAILABLE--" | escape_once | strip_newlines | truncatewords: 10 }}</dt>

    <dd>page.date (custom, set in frontmatter though docs indicate this should be created automatically)</dd>
    <dt>{{ page.date | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.description</dd>
    <dt>{{ page.description | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.dir (actual Jekyll Pages only)</dd>
    <dt>{{ page.dir | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.draft (collection documents and Posts only)</dd>
    <dt>{{ page.draft | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.excerpt (collection documents and Posts only?)</dd>
    <dt>{{ page.excerpt | default:"--NOT AVAILABLE--" | strip_newlines | truncatewords: 10 }}</dt>

    <dd>page.ext (collection documents and Posts only?)</dd>
    <dt>{{ page.ext | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.id (collection documents and Posts only)</dd>
    <dt>{{ page.id | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.layout</dd>
    <dt>{{ page.layout | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.name (actual Jekyll Pages only)</dd>
    <dt>{{ page.name | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.output (collection documents and Posts only?)</dd>
    <dt>{{ page.output | default:"--NOT AVAILABLE--" | escape_once | strip_newlines | truncatewords: 10 }}</dt>

    <dd>page.path</dd>
    <dt>{{ page.path | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.relative_path (collection documents and Posts only)</dd>
    <dt>{{ page.relative_path | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.slug (collection documents and Posts only)</dd>
    <dt>{{ page.slug | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.testFm (custom, set in frontmatter)</dd>
    <dt>{{ page.testFm | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.title</dd>
    <dt>{{ page.title | default:"--NOT AVAILABLE--" }}</dt>

    <dd>page.url</dd>
    <dt>{{ page.url | default:"--NOT AVAILABLE--" }}</dt>

</dl>

<script>
    (function() {
        // Dump the page object to a JS variable - note we have to strip or escape the html
        var jk_page = {{ page | jsonify | strip_html }};
        console.log('--PAGE (jsonify)--', jk_page)
    })();
</script>

