---
title: How node.send() works
description: 'Some detail not included in the documentation.'
comments: true
---
{% include header.html %}

It is synchronous.
See [this discussion](https://groups.google.com/forum/#!topic/node-red/OCHTT8aA3lk)
for details.

If you have code in a function node _after_ a `node.send()`,
it will not run until all connected nodes have **finished** processing their
sending routines. This is true of nodes further downstream as well.

{% include footer.html %}
