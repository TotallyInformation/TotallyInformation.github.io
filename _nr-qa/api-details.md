---
title: Understanding the Node-RED Runtime API
description: >
    Node-RED exposes an extensive API at runtime. While most of this is _not_ available within your flows, much of it _is_ available to custom nodes.

    Unfortunately, documentation for the API lags behind development. This page documents some of the key things I've learned about
    the API, what it does and how it works.
comments: true
date: 2018-01-11 22:00:00
updated: 2018-01-11 22:00:00
---

The official API documentation is [here in the Node-RED docs](https://nodered.org/docs/api/runtime/api).

The API is accessed via the `RED` object.

# Available in Function Nodes

* `RED.util.xxxx`

# Available in Custom Nodes

## RED.util.

* RED.util.cloneMessage(msg)

  See: https://groups.google.com/forum/#!searchin/node-red/RED.util/node-red/dOrpj2S_hi0/X0LthFt0BAAJ
  "For a variety of reasons, you cannot use the JSON.stringfy/parse trick on messages that originate from the HTTP In node."
