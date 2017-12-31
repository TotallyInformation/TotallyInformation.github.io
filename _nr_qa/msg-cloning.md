---
title: 'How to create a clone of a message object'
description: 'Node-RED passes messages by reference not copy. Sometimes though, a copy is needed.'
comments: true
---
{% include header.html %}

By default, Node-RED tries to stay as efficient as possible when "passing" messages from one node to the next. In order to do so, it passes the msg object [by reference](https://hackernoon.com/grasp-by-value-and-by-reference-in-javascript-7ed75efa1293). This is the default for JavaScript.

However, this can sometimes create issues. If, for example, you attach 2 downstream nodes, each gets the _same_ msg object, not a different one. Under certain circumstances, this can lead to unintended consequences where one part of a flow affects another.

In order to avoid this, a function node may create entirely new msg variables and output those. For example:

```javascript
const msg1 = {}

// Note that a message typically contains the important
// data on the payload property
msg1.payload = 'This is a new msg'

// Lets pass through the topic value from the incoming msg
msg1.topic = msg.topic

// This outputs 2 messages. The second is totally
// separate to the first
return [msg, msg1]
```

Alternatively, you can use a utility function `RED.util.cloneMessage(msg)`. For example:

```javascript
// msg1 is a complete clone of msg
const msg1 = RED.util.cloneMessage(msg)

// Lets delete a property from msg1
delete msg1._msgid

// Note that, if we had done `const msg1 = msg`
// deleting the property on msg1 would have deleted
// the property on msg as well since they are actually
// the same object.
//
// By cloning the msg, we created an entirely new msg
// not linked to the old one.

// This outputs 2 messages. The second is totally
// separate to the first
return [msg, msg1]
```


{% include footer.html %}
