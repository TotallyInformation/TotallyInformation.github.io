---
title: How node.send() works
description: 'Some detail not included in the documentation.'
comments: true
---

It is synchronous. See [this discussion](https://groups.google.com/forum/#!topic/node-red/OCHTT8aA3lk) for details.

If you have code in a function node _after_ a `node.send()`, it will not run until _all_ downstream connected nodes have **finished** processing their
sending routines. This is true of nodes further downstream as well.

Also see [Issue 833 on GitHub](https://github.com/node-red/node-red/issues/833)

Another impact of this is that, because [msg's may be passed by reference]({{ site.baseurl }}{% link _nr_qa/msg-cloning.md %}) to downstream nodes, a subsequent node may alter the msg. This means that any processing after a `node.send(msg)` may be getting a msg object that has been altered.
