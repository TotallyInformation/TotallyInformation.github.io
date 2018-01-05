---
title: Node-RED - debugging flows
description: Some hints for debugging Node-RED flows
comments: true
date: 2018-01-03 14:00:01
---

Note: Debugging <i>flows</i> is not the same as debugging <i>nodes</i>.
If you are writing nodes, you will have access to and will need richer tools than are needed here.

* Add more debug nodes. Place a debug node after each point in the flow if needed. Especially place them before as well as after any node   (perhaps a function node) that is trying to change the msg content.

* Change the node name settings for each function and debug node so that it is obvious which node errors and debug output are from even in an image.

* Inside a function that is giving an error, you might need additional debugging output in order to identify where in the function code the error occurs. You can use <code>node.log('position 1')</code>, <code>node.warn('position 1')</code> or similar statements throughout the code.

* Remember that msg's are passed by <i>reference</i> much of the time, see the article on how `node.send()` works [here]({% link _nr_qa/how-node-send-works.md %}) for more details.

* Consider changing the [logging level](https://nodered.org/docs/user-guide/logging) in your `settings.js`. Turning on the audit messages can also help with some issues. It is worth doing this when things are working normally as well so that you get a feel for what is normal and what isn't. You have to restart Node-RED after making changes to `settings.js`.

### Debugging Node-RED admin page, Dashboard and webpage issues

Debugging web pages delivered by Node-RED can be especially challenging for beginners. The main thing to remember is that things are being processed in **two** places. In Node-RED (sometimes referred to as "the server" or "the back end") which happens on your server device; and in the browser (sometimes referred to as "the client" or "the front end"). You will want to become familiar with what happens where so that you know where to look for information.

Node-RED (server, back-end) issues are covered above.

For browser (client, front-end) issues, the following hints may be helpful:

* The developer tools in the browser is the important place to go for debugging information. You can access this via the menu of your favorite browser or by pressing <kbd>F12</kbd>. There is a lot of highly technical information here but I highlight a few key things below.

* The developer tools _console_ tab is the place to view any error or warning log output. Some Node-RED nodes that deliver data to the browser may have optional debug flags that can be turned on that may give more information, node-red-contrib-uibuilder is one such node.

* The network tab will help you to see if you are getting connection problems. Particularly check for repeatedly failing websocket connections. Websockets are used to communicate between the back and front ends. You can also check the response headers there to see if you are getting what you expect.

* The elements tab will show you the current HTML & the style information applied. It also shows things changing and you can even edit things dynamically which is useful when trying to debug layout issues.

### Additional Resources

* [Logging events in Node-RED function nodes](https://nodered.org/docs/writing-functions#logging-events) from the official Node-RED documentation.
* [Controlling console logging output levels](https://nodered.org/docs/user-guide/logging) again from the official Node-RED documentation.
* [Using the Visual Studio Code debugger to debug Node-RED issues]({% link _nr_qa/vscode-debugger.md %}) from this web site. This is much more serious stuff!

Anything I've missed, got wrong or isn't clear? Please leave comments below.
