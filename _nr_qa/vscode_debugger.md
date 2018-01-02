---
title: Node-RED - How to use the Visual Studio Code debugger
description: Using the Visual Studio Code debugger to debug Node-RED issues
comments: true
date: 2018-01-02 22:00:00
---

Originally from [a thread in the Node-RED Google Group](https://groups.google.com/forum/#!topic/node-red/XWTwQyCs4j0).

When using [Visual Studio Code](https://code.visualstudio.com/), the free code editor and Integrated Development Environment (IDE) from Microsoft to develop new nodes, it has an excellent built-in debugger. Since VS Code is built on Node.JS as is Node-RED, you can also use it to debug Node-RED issues.

1. Open the `~/.node-red` (`userDir`) folder, click on the `debugging` sidebar and click on the cog icon which creates and opens a new file called `launch.json` in a folder called `./.vscode`.

2. Add the following configuration to the `.vscode` file:



3. Start up Node-RED then click on the green "start debugging" icon in VSCode, you will get a popup to choose the active process you want to debug. All Node.js based processes will be listed. Pick the one that is Node-RED.

4. Having chosen, the debugger may pause things, if so, click on the continue icon or press F5.

To debug your own (or someone else's) node, expand the "Loaded Scripts" list in the sidebar, find the appropriate script file, click on it to open. Now you can set breakpoints by clicking to the left of the line numbers.

When the execution hits a breakpoint, you will see the current call stack, variables and more.

I seriously wish that I'd got this working before because it is massively useful whether you are creating your own nodes or trying to track down performance or resource issues in your flows.

There are more VScode hints and information in the [VSCode section of this site](/vscode/).
