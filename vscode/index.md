---
title: VSCode Hints and Tips
description: >
    Visual Studio Code is an Integrated Development Environment (IDE) or code editor that is open source and actively developed by Microsoft.
    These articles contain ideas, hints, tips and knowledge about it and how to use it.
comments: true
---

{% include page_lister.html dir="/vscode/" %}

There is also a hint on [how to use VSCode to debug Node-RED]({% link _nr_qa/vscode_debugger.md %}) (and Node.JS) code in the [Node-RED section](/nr_qa) of this site.

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
