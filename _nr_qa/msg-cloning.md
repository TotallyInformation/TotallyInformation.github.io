---
---
By default, Node-RED tries to stay as efficient as possible when "passing"
messages from one node to the next.

In order to do so, it passes the msg object by reference. This is the default for
JavaScript anyway.

However, this can sometimes create issues. If, for example, you attach 2 downstream nodes,
each gets the _same_ msg object, not a different one. Under certain circumstances, this
can lead to unintended consequences where one part of a flow affects another.

In order to avoid this, a function node may create entirely new msg variables and output
those. Alternatively, you can use a utility function `RED.util.cloneMessage(msg)`.

{% include footer.md date=page.date %}
