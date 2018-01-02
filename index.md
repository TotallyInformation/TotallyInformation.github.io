---
title: Home
description: 'This site is used to publish development-related reference information curated by Totally Information.'
someData: Some Text for someData
comments: false
date: 2018-01-02 14:20:00
---

## Node-RED Questions and Answers

Node-RED is a flow-based (visual) programming tool. These pages have some information that may be currently missing from the documentation.

{% include collection_doc_lister.html collection=site.nr_qa %}

## GitHub Pages and Jekyll/Liquid

Hints and tips on using Jekyll for publishing to GitHub Pages.

{% include collection_doc_lister.html collection=site.github_pages %}

## Totally Information's Public Code Repositories

<table>
    {% assign repos =  site.github.public_repositories | sort: "name" %}
    {% tablerow repository in repos cols:1 %}
        <a hre="{{ repository.html_url }}">{{ repository.name }}</a>
    {% endtablerow %}
</table>

<script>
    (function() {
        // Dump the page object to a JS variable - note we have to strip or escape the html
        var jk_page = {{ page | jsonify | strip_html }};
        //var someData = '{{ page.someData }}'
        //var layout = '{{ layout }}'
        var github = '{{ site.github | jsonify }}'
        console.log('--PAGE (jsonify)--', jk_page)
        //console.log('someData', someData)
        console.log('--SITE.GITHUB (jsonify)--', github)
    })();
</script>
