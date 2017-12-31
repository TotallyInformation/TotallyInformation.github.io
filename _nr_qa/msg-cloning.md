---
title: 'How to create a clone of a message object'
description: 'Node-RED tried to pass messages by reference not copy. Sometimes though, a copy is needed.'
comments: true
---
{% include header.html %}

By default, Node-RED tries to stay as efficient as possible when "passing" messages from one node to the next. In order to do so, it passes the msg object [by reference](https://hackernoon.com/grasp-by-value-and-by-reference-in-javascript-7ed75efa1293). This is the default for JavaScript.

These are the rules for when msg's are cloned:
- If a node is wired to another single node, the message is **not** cloned.
- If a node is wired to **two** or more nodes from a single output of the parent node, the message is cloned to all except the 'first' of the output nodes. Put another way, downstream nodes 2-n get a copy of the msg object.

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

In some cases, you might also consider the [node-red-contrib-diode](https://www.npmjs.com/package/node-red-contrib-diode) node created by Pete Scargill. That provides a node that enforces a clone.

Also note that cloning a large msg object can take a few hundred milliseconds, not inconsiderable if you are handling large volumes of messages.

See also the [page on how `node.send()` works]({{ site.baseurl }}{% link _nr_qa/how_node-send_works.md %}) as that is also impacted by this issue.

Finally, here are some words on the subject by Node-RED author Nick O'Leary in [GitHub Issue 1214](https://github.com/node-red/node-red/issues/1214):

> We can discuss what behaviour is expected or otherwise, but here are the facts of the current behaviour which explains what you observe.
>
> 1. the runtime tries to avoid cloning messages where possible as it is an expensive operation. If a node calls send with a single message object and is wired to just one other node, it will skip automatically cloning the message as, in theory, there is only the single copy to worry about.
>
> 2. the runtime has no way of knowing that a subsequent call to send is with a reused message object.
>
> 3. the message passing is 'depth-first' up to the point an async operation is encountered.
>
> 4. in a flow consisting of Inject -> Function -> Debug, the call to send(msg) from the Function node triggers the Debug node which displays the message. The call stack then unwinds and allows the Function node to make its next call to send. Because the original msg has already been dealt with by the Debug node, the fact the msg has been modified is not noticed.
>
> 5. In a flow consisting of Inject -> Function -> Delay -> Debug, the Delay node contains an async action. This means the Function node is able to make multiple calls to send - queuing up all of its messages at the Delay node. They then, asynchronously, arrive at the Debug node where you discover what your thought were multiple individual messages were in fact multiple references to the same message object.
>
> ### So what do we do about it?
> We've always said that a node is responsible for cloning a message object if they intend to reuse it multiple times. That is something that is probably more explicit in the nodes we publish than in the docs (which is why I marked this as a docs issue in my original response).
>
> That said, it will be an uphill struggle to explain this nuance of message passing to all users - especially those who don't read the docs.
>
> ### In summary:
> * we avoid cloning in order to improve performance on the most common flow case (one node, one message, one recipient).
> * users who have wanted to implement the edge case of sending multiple messages (and reuse the object) have either diligently cloned their message or not.
> * if we add cloning to all messages it will penalise all users - and those that have been diligent doubly so as their code will clone the message and then so will ours.

{% include footer.html %}
