---
title: Alternate static site generators to Jekyll
description: >
    Jekyll is built on Ruby which is a pain to work with on Windows. It also doesn't match my chosen development language of JavaScript.

    Here are some possible alternatives to Jekyll along with some strengths and weaknesses.
comments: true
date: 2018-01-07 22:00:00
---

## Node.JS Based

All of this list use Node.JS to underpin the generation of static pages.

* Hexo - seemed promising at first sight. But a quick dig through the impressive list of addins reveals many of them to be out of date or not working properly.

  - &#x2705; Uses YAML configuration and Front Matter like Jekyll does.
  - &#x2705; Large list of plugins, all published on NPM.
  - &#x2705; Simple initial set-up.
  - &#10060; Many plugins out-of-date or not fully working.
  - &#10060; Many plugins very poorly documented or documented in Chinese.
  - &#10060; Does not handle page-based sites well. Focussed on blogging (posts).
  - &#10060; Needs plugins to provide more rounded capabilities.

* Gatsby - Complex but comprehensive

  - &#x2705; [Large list of plugins published on NPM](https://www.npmjs.com/browse/keyword/gatsby) & [GitHub](https://github.com/topics/gatsby).
  - &#x2705; Large number of contributors.
  - &#x2705; Powerful.
  - &#10060; Complex to configure, lots of moving parts.
  - &#10060; An aweful lot of "magic" happens! While the file system is reasonably well arranged it is very hard to reason about the logic.
