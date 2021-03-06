---
title: 'How to use the ACE editor in the Function node'
description: 'The ACE editor is used in the function node to help you create well-formed JavaScript. However, there are now a number of different versions of JavaScript and it is sometimes helpful to use slightly different styles of coding to the defaults.'
comments: true
---

[ACE](https://ace.c9.io/) uses JSHint which allows you to override settings locally.

Use something like the following at the start of your Function code:

```javascript
/*jshint sub:true,asi:true,maxerr:1000*/
```

which would, for example:

- Turn off the warnings about using subscripting to access object properties.
- Turn off warnings about missing semi-colons.
- Continue processing warnings & errors up to 1,000 lines instead of the default 50.

See the [JSHint documentation](http://jshint.com/docs/options/) for more information.
