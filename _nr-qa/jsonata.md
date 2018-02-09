---
title: Using JSONata to transform msg data
description: >
    JSONata is a syntax designed by IBM to help reformat and restructure JSON data in a similar vein to the way that XLST is used to transform XML data.
comments: true
date: 2018-01-02 22:00:00
---

JSONata is available in Node-RED via the _change_ node. It is available where you see the &Integral; symbol.

See the [JSONata home page](http://jsonata.org/) for details of the standard. There is also a useful [exerciser page](http://try.jsonata.org/HJfrIhQEf) that lets you paste in some source JSON data, a JSONata formula and see the output. Note, however, that the exerciser page tends to be using a newer version of the library than Node-RED uses.

# Examples

## General

### Global and Flow context variables

    ( {
      "aGlobalVariable": $globalContext('globalVariableName'),
      "aFlowVariable": $flowContext('flowVariableName')
    } )

### Extract specific element from a context variable
This shows how to extract an element from a context variable based on the msg.topic and assuming you have a variable that is keyed on topic. An example might be a global variable keyed on sensor ID that contains sensor locations where you want to add the location to the data from the sensor to pass on to MQTT.

    {
       "location": $globalContext('locations["' & topic & '"]'),
       "origPayload": payload
    }

### Accessing msg
Object entities are exposed directly as names, e.g. msg.payload is accessed simply as "payload". If you want to access the whole msg object, you need to use the "$" variable at the top level of your expression. e.g.

    $._msgid

would return the unique message id. If you end up using functions or other enclosures, you can use $$ to access the top level object.

### Complex example

Extracts the keys and values from the original msg, copies the original msg.payload, adds some extra object data and a timestamp. Illustrates the use of functions and variables.

```
(
    $lookupVals := function($i) { $lookup($$, $i) };
    $origMsgVals := $map($keys($$), $lookupVals);
    {
      "meta": { "a": 1, "thisIs":"more data" },
      "origPayload": payload,
      "origMsgKeys": $keys($$),
      "origMsgVals": $origMsgVals,
      "timestamp": $now()
    }
)
```

## Change Node Recipes

### Add extra levels to the start of a topic
Use Set against msg.topic and use the following JSONata expression:

    "EXTRA/LEVELS/" & topic

### Recreate the msg on the msg.payload

The previous examples are now outdated as of Jan 2018 with the introduction of Node-RED v0.18. This saw an ugrade to JSONata v1.5 which brings a few other possibilities.

    {
      "topic": topic,
      "payload": $,
      "_msgid": _msgid
    }

The above will work just fine. However, if the original message originates from an http-in node or similar, it will contain circular references and a LOT of data and will cause issues.
To avoid this, use the new `$clone()` function. Node-RED overloads this with a safe version that avoids the issues at least with http-in messages.

    {
      "topic": topic,
      "payload": $clone($),
      "_msgid": _msgid
    }

For reference, here is the previous suggestion:

Sometimes you might want to move the content of the msg to msg.payload. For example, if you wanted to send the msg as a debug to MQTT. You cannot do this directly (setting msg.payload to $) as the system thinks this is a circular reference and blocks it. So you have to recreate the msg manually. I include a list of the msg's keys so that you can know if you missed anything.

    {
      "topic": topic,
      "payload": payload,
      "_msgid": _msgid,
      "msgKeys": $keys($),
    }

There are undoubtedly other ways to do this in a more automated way.

Taking this slightly further, you can also get the keys and values of the original msg in an array:

```
(
    $spread($)
)
```

## Reprocess a payload containing an array

How to process an array/object combination plus calculate a true/false flag from 2 properties in the array.

Example input msg:
```json
{
    "payload": [
        {
            "Station ID": "3015432",
            "Station Name": "Sg.Klang di Tmn Sri Muda 1",
            "District": "Petaling",
            "River Basin": "Sg.Klang",
            "Last Update": "30/12/2017 01:30",
            "River Level (m)": "1.13",
            "Normal": "2.80",
            "Alert": "4.40",
            "Warning": "4.58",
            "Danger": "5.00"
        },
        {
            "Station ID": "3015490",
            "Station Name": "Sg.Damansara di TTDI Jaya",
            "District": "Petaling",
            "River Basin": "Sg.Klang",
            "Last Update": "30/12/2017 01:30",
            "River Level (m)": "13.35",
            "Normal": "3.50",
            "Alert": "7.30",
            "Warning": "7.80",
            "Danger": "8.30"
        }
    ]
}
```

Example JSONata to reformat the output
```
payload.(
	{
		"station": $.'Station ID',
    	"name": $.'Station Name',
        "level": $.'River Level (m)',
        "alert": $number($.'River Level (m)') > $number($.Alert) ? true : false
    }
)
```
Produces:
```json
[
  {
    "station": "3015432",
    "name": "Sg.Klang di Tmn Sri Muda 1",
    "level": "1.13",
    "alert": false
  },
  {
    "station": "3015490",
    "name": "Sg.Damansara di TTDI Jaya",
    "level": "13.35",
    "alert": true
  }
]
```
See https://groups.google.com/forum/#!topic/node-red/J0020rPetAY for more information.
