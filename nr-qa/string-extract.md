---
title: How to extract a substring from the msg object
description: >
    Node-RED passes a msg object between node instances in a flow. It is common to need to extract
    a sub-string and there are multiple ways to achieve this. Here are some simeple examples.
comments: true
date: 2018-01-04 12:00:00
updated: 2018-01-04 12:00:00
---

The two main methods to use are either a "Change" node or a "Function" node.

The examples below simply extract the first 4 characters of the `msg.payload` property to illustrate the possibilities.

## Change Node Examples

This is the simplest and generally the preferred approach unless you need to do a large number of changes to a msg.

### Regular Expression Method

Extracting a sub-string is easily done using a regular expression which is one of the methods available to a "Change Rule" in the Change node.

* Change Rule Type: Change, msg.payload
* Search For: `^(.{4}).*$`
* Replace With: `$1`

This regex matches the whole `msg.payload` string. The `()` brackets in a regular expression allow the wrapped content specification to be captured to an internal variable which we use in the replace section as `$1`

```
[{"id":"4a1695b4.85f22c","type":"inject","z":"20e74d1e.f19692","name":"","topic":"","payload":"21.300000<xml><exec>/tclrega.exe</exec><sessionId></sessionId><httpUserAgent></httpUserAgent><d>BidCos-RF.MEQ0800939:4.ACTUAL_TEMPERATURE</d></xml>","payloadType":"str","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":270,"y":100,"wires":[["235a041.55c49fc"]]},{"id":"cbbf5f80.6b42a","type":"debug","z":"20e74d1e.f19692","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":630,"y":100,"wires":[]},{"id":"235a041.55c49fc","type":"change","z":"20e74d1e.f19692","name":"","rules":[{"t":"change","p":"payload","pt":"msg","from":"^(.{4}).*$","fromt":"re","to":"$1","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":440,"y":100,"wires":[["cbbf5f80.6b42a"]]}]
```

### JSONata Methods

There are several ways to use [JSONata](http://jsonata.org/) to extract a sub-string, here I illustrate a couple

#### $match (regular expression)

* Change Rule Type: Set, msg.payload
* To: `$match(payload, /^.{4}/)`

```
[{"id":"61aeec2b.95a4f4","type":"change","z":"20e74d1e.f19692","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"$match(payload, /^.{4}/)\t","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":420,"y":160,"wires":[["9e768e8.0e2e67"]]},{"id":"ae3d0976.e0a4a8","type":"inject","z":"20e74d1e.f19692","name":"","topic":"","payload":"21.300000<xml><exec>/tclrega.exe</exec><sessionId></sessionId><httpUserAgent></httpUserAgent><d>BidCos-RF.MEQ0800939:4.ACTUAL_TEMPERATURE</d></xml>","payloadType":"str","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":270,"y":160,"wires":[["61aeec2b.95a4f4"]]},{"id":"9e768e8.0e2e67","type":"debug","z":"20e74d1e.f19692","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":630,"y":160,"wires":[]}]
```

#### $substring

* Change Rule Type: Set, msg.payload
* To: `$substring(payload, 0, 4)`

```
[{"id":"f550ddc8.1a56f","type":"change","z":"20e74d1e.f19692","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"$substring(payload, 0, 4)\t","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":420,"y":220,"wires":[["835a813f.698f1"]]},{"id":"1bdcf392.de1a5c","type":"inject","z":"20e74d1e.f19692","name":"","topic":"","payload":"21.300000<xml><exec>/tclrega.exe</exec><sessionId></sessionId><httpUserAgent></httpUserAgent><d>BidCos-RF.MEQ0800939:4.ACTUAL_TEMPERATURE</d></xml>","payloadType":"str","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":270,"y":220,"wires":[["f550ddc8.1a56f"]]},{"id":"835a813f.698f1","type":"debug","z":"20e74d1e.f19692","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":630,"y":220,"wires":[]}]
```

## Function Node Example

The Function node allows you to use JavaScript to process the msg object. You may wish to use this method if you
are familiar with JavaScript and have a fairly complex processing requirement or an existing set of code.

```javascript
/***
 * This demonstrates a number of ways to extract
 * a sub-string from the payload.
 *
 * Note that you don't really need an intermediate
 * variable but it does make the code easier to read.
 *
 * Also note that I've used `const` which was
 * introduced to JavaScript in ES2015 (AKA ES6).
 ***/

// slice is the easiest method to extract a sub-string
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice
let newStr = msg.payload.slice(0, 4);

// ---- Other possible methods ---- {

// Regular Expressions: str.match, str.exec or str.replace
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace

// str.substr - older method than slice
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr
//let newStr = msg.payload.substr(0,4)
// str.substring - older method than slice
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr
//let newStr = msg.payload.substring(0,3)
//
// The arguments of substring() represent the starting and ending indexes, while the arguments of
// substr() represent the starting index and the length of characters to include in the returned string.

// -------------------------------- }

msg.payload = newStr;

return msg;
```

```
[{"id":"da10e728.ae30d8","type":"inject","z":"20e74d1e.f19692","name":"","topic":"","payload":"21.300000<xml><exec>/tclrega.exe</exec><sessionId></sessionId><httpUserAgent></httpUserAgent><d>BidCos-RF.MEQ0800939:4.ACTUAL_TEMPERATURE</d></xml>","payloadType":"str","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":270,"y":280,"wires":[["7b9217ad.a29e58"]]},{"id":"d49e42b1.f141d","type":"debug","z":"20e74d1e.f19692","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":630,"y":280,"wires":[]},{"id":"7b9217ad.a29e58","type":"function","z":"20e74d1e.f19692","name":"","func":"/***\n * This demonstrates a number of ways to extract\n * a sub-string from the payload.\n * \n * Note that you don't really need an intermediate\n * variable but it does make the code easier to read.\n * \n * Also note that I've used `const` which was \n * introduced to JavaScript in ES2015 (AKA ES6).\n ***/\n\n// slice is the easiest method to extract a sub-string\n// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice\nlet newStr = msg.payload.slice(0, 4);\n\n// ---- Other possible methods ---- {\n\n// Regular Expressions: str.match, str.exec or str.replace\n// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match\n// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec\n// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace\n\n// str.substr - older method than slice\n// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr\n//let newStr = msg.payload.substr(0,4)\n// str.substring - older method than slice\n// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr\n//let newStr = msg.payload.substring(0,3)\n//\n// The arguments of substring() represent the starting and ending indexes, while the arguments of \n// substr() represent the starting index and the length of characters to include in the returned string.\n\n// -------------------------------- }\n\nmsg.payload = newStr;\n\nreturn msg;","outputs":1,"noerr":0,"x":390,"y":280,"wires":[["d49e42b1.f141d"]]}]
```
