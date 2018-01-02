---
title: 'GitHub Pages and Jekyll/Liquid'
description: 'Hints and tips on using Jekyll for publishing to GitHub Pages.'
comments: false
---

{% include collection_doc_lister.html collection=site.github_pages %}

<script>
    (function() {
        //var collection = {{ site.github_pages | jsonify | strip_html }};
        //console.log('--COLLECTION (jsonify)--', collection)
    })();
</script>
