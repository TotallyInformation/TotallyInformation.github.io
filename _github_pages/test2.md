---
title: A Test Jekyll/GitHub Collection Document (MarkDown)
description: Testing collections and page variables - this page is in a collection and is a markdown file
comments: test
testFm: testing front matter variables
date: 2018-01-02 17:10:10
---

<h1>This is a {% if page.collection %}COLLECTION DOCUMENT{% else %}JEKYLL PAGE{% endif %}</h1>

{{ include test_page_vars }}
