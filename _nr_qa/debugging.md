
title: Node-RED - debugging flows
description: Some hints for debugging Node-RED flows
comments: true
date: 2018-01-02 22:00:00


> Note: Debugging <i>flows</i> is not the same as debugging <i>nodes</i>.
If you are writing nodes, you will have access to and will need richer tools than are needed here.

* Add more debug nodes. Place a debug node after each point in the flow if needed. Especially place them before as well as after any node   (perhaps a function node) that is trying to change the msg content.

* Change the node name settings for each function and debug node so that it is obvious which node errors and debug output are from even in an image.

* Inside a function that is giving an error, you might need additional debugging output in order to identify where in the function code the error occurs. You can use <code>node.log('position 1')</code>, <code>node.warn('position 1')</code> or similar statements throughout the code.

* Remember that msg's are passed by <i>reference</i> much of the time, see the [article on how `node.send()` works]({% link how_node-send_works.md %}) for more details.

### Additional Resources

* [Logging events in Node-RED function nodes](https://nodered.org/docs/writing-functions#logging-events) from the official Node-RED documentation.
* [Controlling console logging output levels](https://nodered.org/docs/user-guide/logging) again from the official Node-RED documentation.
* [Using the Visual Studio Code debugger to debug Node-RED issues]({% link vscode_debugger.md %}) from this web site. This is much more serious stuff!
