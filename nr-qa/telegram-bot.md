---
title: Secure Home Automation Controls via a Telegram Bot
description: >
    Shows how to create a secure set of home automation controls without exposing Node-RED to the Internet.

    It uses a Telegram bot with a number of pre-defined commands.
comments: true
date: 2018-02-11 17:00:00
---

Connecting any web application server securely to the Internet can be a challenge.
Doing so on a shoestring with low-cost open source hardware and software even more so,
especially when you don't have security experts to call upon.

One way to avoid these issues is to make use of a secure messaging service that supports
"bots" and end-to-end encryption. That is what the following flows use.

As you will see, writing a decent bot, even with Node-RED helping, can also be a challenge,
there's a lot to think about in terms of the flow logic if you don't want to leave your
users stranded in the middle of a conversation with the bot.

This set of flows aims to illustrate a simple bot that will let you see the status of a
set of wireless controlled lights or switches. It will also let you turn them on or off remotely.

## Prerequisites

* An account on the [Telegram](https://telegram.org/) messaging service.
* An [API ID](https://core.telegram.org/api/obtaining_api_id).
* [Node-RED](https://nodered.org/) - I am using v0.18 with Node.js v8.9 - I developed this on Windows 10, any supported platform should be fine.
* An MQTT broker.

  The example uses MQTT to issue commands to MQTT. You will need to have existing logic to convert the MQTT messages into commands that control
  your hardware. I use an existing Node-RED based home automation setup with a mix of wireless switch devices.

* The following Node-RED nodes will also need to be installed:
  * [node-red-contrib-telegrambot](https://flows.nodered.org/node/node-red-contrib-telegrambot) -
    A simple set of nodes to interact with the API. This is the heart of the "bot".
  * [node-red-contrib-rive](https://flows.nodered.org/node/node-red-contrib-rive) - A [Rivescript](https://www.rivescript.com) implementation.
    Rivescript allows a richer set of interactions with users without having to generate large amounts of complex code.

## The MQTT Message Schema

I have a very simple schema for monitoring and issuing commands to my remote switches.

The topic structure is:

    COMMAND/SWITCHnn

Where "nn" is the number assigned to the switch device.

The payload structure is even simpler. It is one of `On` or `Off`. Text case is not relevant.

A flow in Node-RED listens to changes on the topics and translates the device to a physical device output.

I use the `retain` flag on the messages so that, on startup of any service that is subscribed to `COMMAND/#`,
that service will receive all of the current switch statuses.

These flows subscribe as above and maintain an in-memory copy of the statuses.

## Switch Metadata

Since `SWITCH01`, etc. is not terribly friendly, I also maintain an in-memory set of data, keyed on the switch number.
This is used to provide friendly names and location information as needed. An example set of data is included in the
first of the two flows.

## Flow

![Startup flow](/assets/images/telegrambot-setup.PNG)

![Get status flow](/assets/images/telegrambot-getStatus.PNG)

![Process commands flow](/assets/images/telegrambot-processCmds.PNG)

Import this flow to Node-RED.

Make sure that The 'Save Switch Metadata' is on the first tab of your flows so that it is executed early. Alternatively,
add it to your `settings.js` file.

{% raw %}
```json
[
    {
        "id": "af703fcd.0f9d1",
        "type": "mqtt in",
        "z": "25a82b05.7c01a4",
        "name": "",
        "topic": "COMMAND/#",
        "qos": "1",
        "broker": "1a1b7cff.9511b3",
        "x": 150,
        "y": 280,
        "wires": [
            [
                "76000fb1.127cd"
            ]
        ]
    },
    {
        "id": "dbc61113.e382b",
        "type": "link out",
        "z": "25a82b05.7c01a4",
        "name": "",
        "links": [
            "b572c9cf.b6d518",
            "bb2d9017.65d8f"
        ],
        "x": 895,
        "y": 260,
        "wires": []
    },
    {
        "id": "f901c27e.ae96c",
        "type": "batch",
        "z": "25a82b05.7c01a4",
        "name": "",
        "mode": "interval",
        "count": 10,
        "overlap": 0,
        "interval": "1",
        "allowEmptySequence": false,
        "topics": [],
        "x": 493,
        "y": 280,
        "wires": [
            [
                "59f6138c.12ef1c"
            ]
        ]
    },
    {
        "id": "dedee945.16fa88",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "x": 930,
        "y": 300,
        "wires": []
    },
    {
        "id": "59f6138c.12ef1c",
        "type": "join",
        "z": "25a82b05.7c01a4",
        "name": "",
        "mode": "auto",
        "build": "string",
        "property": "payload",
        "propertyType": "msg",
        "key": "topic",
        "joiner": "\\n",
        "joinerType": "str",
        "accumulate": false,
        "timeout": "",
        "count": "",
        "reduceRight": false,
        "reduceExp": "",
        "reduceInit": "",
        "reduceInitType": "",
        "reduceFixup": "",
        "x": 613,
        "y": 280,
        "wires": [
            [
                "447ddef5.9d985"
            ]
        ]
    },
    {
        "id": "76000fb1.127cd",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "Expand Payload",
        "func": "/**\n * Merge topic and payload into the payload\n * so that we get something like 'COMMAND/SWITCH01: On'\n **/\n\n// Get the switch metadata\nconst switchLocations = global.get('switchLocations')\nconst switchId = msg.topic.replace('COMMAND/','')\nconst cmd = msg.payload.padStart(3)\n\nconst switchName = switchLocations[switchId] ? switchLocations[switchId].description : switchId\n\n//msg.payload = `${switchName}: ${msg.payload}`\nmsg.payload = `\\`${cmd} :: ${switchName}\\``\n\n// Also save the switch status in memory to allow replay\nlet cmdStatus = flow.get('cmdStatus') || {}\ncmdStatus[switchName] = msg.payload\nflow.set('cmdStatus', cmdStatus)\n\nmsg.topic = 'COMMAND'\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 320,
        "y": 280,
        "wires": [
            [
                "f901c27e.ae96c"
            ]
        ]
    },
    {
        "id": "447ddef5.9d985",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "Merge Payload",
        "func": "/**\n * Join the payload array ready for sending to Telegram\n * Note that the Telegram output link also prepends the topic\n **/\n\nif ( msg.payload.length > 1 ) {\n    msg.topic = 'Current Switch Statuses'\n} else {\n    msg.topic = 'Switch Status Change'\n}\n\n//msg.payload = \"```&#160;\" + msg.payload.join(\"\\n\") + \"```\"\nmsg.payload = msg.payload.join(\"\\n\")\nmsg.parse_mode = 'Markdown'\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 763,
        "y": 280,
        "wires": [
            [
                "dedee945.16fa88",
                "dbc61113.e382b"
            ]
        ]
    },
    {
        "id": "a87cf9c5.c47098",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "replay cache",
        "func": "let cmdStatus = flow.get('cmdStatus') || {}\n\nfor (const switchName in cmdStatus) {\n  node.send( { topic: switchName, payload: cmdStatus[switchName] } )\n}",
        "outputs": 1,
        "noerr": 0,
        "x": 310,
        "y": 360,
        "wires": [
            [
                "a68ca9d4.01a438",
                "f901c27e.ae96c"
            ]
        ]
    },
    {
        "id": "73fb66b6.b19878",
        "type": "telegram command",
        "z": "25a82b05.7c01a4",
        "name": "",
        "command": "/lights",
        "bot": "d14808a8.d291e8",
        "x": 130,
        "y": 360,
        "wires": [
            [
                "a87cf9c5.c47098"
            ],
            []
        ],
        "outputLabels": [
            "Authorised + Matches Command",
            "Authorised - No command match"
        ]
    },
    {
        "id": "a68ca9d4.01a438",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "x": 510,
        "y": 360,
        "wires": []
    },
    {
        "id": "10c72c6a.4a8ed4",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "x": 950,
        "y": 600,
        "wires": []
    },
    {
        "id": "b7702c50.7d825",
        "type": "telegram command",
        "z": "25a82b05.7c01a4",
        "name": "",
        "command": "/light",
        "bot": "d14808a8.d291e8",
        "x": 130,
        "y": 680,
        "wires": [
            [
                "5cbce84f.0c7038"
            ],
            []
        ],
        "outputLabels": [
            "Authorised + Matches Command",
            "Authorised - No command match"
        ]
    },
    {
        "id": "349c448e.28629c",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "Process Rivescript response",
        "func": "const newPay = msg.origPay\nlet out = 0 // 0=command - goes to MQTT, 1 = msg back to bot\nlet payload = msg.payload\nlet cmd = ''\nlet msgId = msg.origPay.messageId\n\nfunction getMissingInfo(infoStr) {\n    out = 1\n    payload = payload.replace(infoStr,'')\n    \n    cmd = msg.originalMessage.text.split(' ')[0]\n    \n    // Save the info so far so we can combine with the reply\n    //pendingCmds(msgId, cmd)\n    \n    //msg.topic = `${cmd}: More information needed`\n    \n    newPay.content = `More information needed ...\\n${payload}`\n    \n    newPay.options = {\n        reply_to_message_id: msgId,\n        reply_markup: JSON.stringify({\n            force_reply: true,\n            selective: true,\n        }),\n        //parse_mode: 'Markdown'\n        /*\n            \n            'keyboard': [[\n                'on', 'off'\n            ]],\n            'resize_keyboard' : true, \n            'one_time_keyboard' : true\n\n            inline_keyboard: [[\n                {\n                    'text': 'On',\n                    'callback_data': 'on'            \n                }, {\n                    'text': 'Off',\n                    'callback_data': 'off'            \n                }\n            ]]\n        */\n    } // -- End of options -- //\n    \n    msg.payload = newPay\n}\n\n// A processable response?\nswitch ( payload.substr(0,4) ) {\n    // All info provided so send to MQTT command output {\n    case '|01,':\n        payload = payload.replace('|01,','')\n        \n        cmd = payload.toLowerCase().split(',')\n        \n        msg.topic = 'COMMAND/SWITCH' + cmd[0].padStart(2,'0')\n        \n        msg.payload = cmd[1]\n        break;\n    // --- End of |01, --- // }\n    \n    // Command given but not the switch number {\n    case '|02,':\n        getMissingInfo('|02,')\n        break;\n    // --- End of |02, --- // }\n\n    // Switch number given but not command (on|off) {\n    case '|03,':\n        getMissingInfo('|03,')\n        break;\n    // --- End of |03, --- // }\n    \n    // {\n    case '|04,':\n        return // exit, no msg sent\n        break;\n    // --- End of |04, --- // }\n\n    // The bot needs more info {\n    default:\n        return // exit, no msg sent\n    // --- End of Default --- // }\n    \n} // ---- End of switch ---- //\n\nif ( out === 0 ) return [msg, null]\nelse return [null, msg]",
        "outputs": 2,
        "noerr": 0,
        "x": 720,
        "y": 660,
        "wires": [
            [
                "10c72c6a.4a8ed4",
                "a90b971e.ef8e98"
            ],
            [
                "6edcd4e6.4e2abc"
            ]
        ],
        "outputLabels": [
            "Known cmd with all info",
            "More info needed"
        ]
    },
    {
        "id": "a90b971e.ef8e98",
        "type": "mqtt out",
        "z": "25a82b05.7c01a4",
        "name": "",
        "topic": "",
        "qos": "1",
        "retain": "true",
        "broker": "1a1b7cff.9511b3",
        "x": 950,
        "y": 640,
        "wires": []
    },
    {
        "id": "a5ccb9cf.543a08",
        "type": "rive",
        "z": "25a82b05.7c01a4",
        "name": "",
        "script": "! version = 2.0\n\n! array cmds = on off\n\n+  # (@cmds)\n- |01,<star1>,<star2>\n\n+  (@cmds) #\n- |01,<star2>,<star1>\n\n+ (@cmds)\n- |02,Which switch/light would you like to turn <star1>? [1-10]\n\n+ #\n- |03,Would you like to turn switch/light <star1> on or off? [on|off]\n\n+ *\n- I'm sorry, I don't understand what you are asking for.\\n\n^ Please use the format: /light # on|off\\n\n^ You can even use: /on #\n",
        "x": 490,
        "y": 680,
        "wires": [
            [
                "28ff679.f879b98",
                "349c448e.28629c"
            ],
            [
                "e82ab2d6.68742"
            ]
        ],
        "outputLabels": [
            "Known response",
            "Unknown response"
        ]
    },
    {
        "id": "28ff679.f879b98",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "x": 650,
        "y": 620,
        "wires": []
    },
    {
        "id": "5cbce84f.0c7038",
        "type": "change",
        "z": "25a82b05.7c01a4",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "origPay",
                "pt": "msg",
                "to": "payload",
                "tot": "msg"
            },
            {
                "t": "change",
                "p": "payload.content",
                "pt": "msg",
                "from": "(\\/light|\\/switch|\\/off|\\/on)",
                "fromt": "re",
                "to": "",
                "tot": "str"
            },
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.content",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 320,
        "y": 680,
        "wires": [
            [
                "a5ccb9cf.543a08",
                "33d717f1.8f92d8"
            ]
        ]
    },
    {
        "id": "2ff563b.8ba669c",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "Save orig cmd",
        "func": "const pendingCmds = flow.get('pendingCmds') || {}\n\npendingCmds[ msg.payload.sentMessageId ] = msg.originalMessage.text\n\nflow.set('pendingCmds', pendingCmds)\n\nreturn msg;\n",
        "outputs": 1,
        "noerr": 0,
        "x": 480,
        "y": 1000,
        "wires": [
            [
                "b362bd1b.a0dc8"
            ]
        ]
    },
    {
        "id": "9d7d5d7f.37b8a",
        "type": "comment",
        "z": "25a82b05.7c01a4",
        "name": "Bot Command List",
        "info": "Use the `/setcommands` command in the botfather channel.\n\n```\nlights - Show the current status of all lights and switches\nswitches - Show the current status of all lights and switches\non - Turn on a switch, provide the switch number\noff - Turn off a switch, provide the switch number\nlight - Turn on and off lights at home\nswitch - Turn on and off switches at home\nhelp - Show help about how to use this bot\nh - Show help about how to use this bot\n```",
        "x": 130,
        "y": 220,
        "wires": []
    },
    {
        "id": "3d77a9f5.4f63d6",
        "type": "telegram command",
        "z": "25a82b05.7c01a4",
        "name": "",
        "command": "/help",
        "bot": "d14808a8.d291e8",
        "x": 130,
        "y": 480,
        "wires": [
            [
                "472a916f.19feb"
            ],
            []
        ],
        "outputLabels": [
            "Authorised + Matches Command",
            "Authorised - No command match"
        ]
    },
    {
        "id": "e8492903.2dea48",
        "type": "link out",
        "z": "25a82b05.7c01a4",
        "name": "Send simple response",
        "links": [
            "b572c9cf.b6d518",
            "bb2d9017.65d8f"
        ],
        "x": 435,
        "y": 500,
        "wires": []
    },
    {
        "id": "472a916f.19feb",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "Return help info",
        "func": "msg.topic = 'Bot Help'\n\nmsg.parse_mode = 'HTML'\n\nmsg.payload = \"There are several commands, type '/' to see them.\\n\"\n\nmsg.payload += `<b>/lights</b> or <b>/switches</b>\n&#160;&#160;&#160;List all showing whether <i>on</i> or <i>off</i>\n<b>/on #</b> or <b>/off #</b>\n&#160;&#160;&#160;Turn on or off switch number #\n<b>/light # [on|off]</b>\n&#160;&#160;&#160;As above (can also use /switch)\n<b>/light [on|off] #</b>\n&#160;&#160;&#160;As above\n<b>/help</b> or <b>/h</b>\n&#160;&#160;&#160;This information`\n\n//msg.payload += \"\\n```\"\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 300,
        "y": 500,
        "wires": [
            [
                "e8492903.2dea48"
            ]
        ]
    },
    {
        "id": "a6cbdbb.fbb7928",
        "type": "telegram command",
        "z": "25a82b05.7c01a4",
        "name": "",
        "command": "/switches",
        "bot": "d14808a8.d291e8",
        "x": 140,
        "y": 420,
        "wires": [
            [
                "a87cf9c5.c47098"
            ],
            []
        ],
        "outputLabels": [
            "Authorised + Matches Command",
            "Authorised - No command match"
        ]
    },
    {
        "id": "410a3017.5bb82",
        "type": "telegram command",
        "z": "25a82b05.7c01a4",
        "name": "",
        "command": "/switch",
        "bot": "d14808a8.d291e8",
        "x": 130,
        "y": 740,
        "wires": [
            [
                "5cbce84f.0c7038"
            ],
            []
        ],
        "outputLabels": [
            "Authorised + Matches Command",
            "Authorised - No command match"
        ]
    },
    {
        "id": "33d717f1.8f92d8",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "x": 470,
        "y": 640,
        "wires": []
    },
    {
        "id": "e82ab2d6.68742",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "Unknown response",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "x": 690,
        "y": 720,
        "wires": []
    },
    {
        "id": "d1a04273.367c9",
        "type": "telegram sender",
        "z": "25a82b05.7c01a4",
        "name": "Ask for more info",
        "bot": "d14808a8.d291e8",
        "x": 290,
        "y": 1000,
        "wires": [
            [
                "2ff563b.8ba669c"
            ]
        ]
    },
    {
        "id": "b362bd1b.a0dc8",
        "type": "telegram reply",
        "z": "25a82b05.7c01a4",
        "name": "Process reply",
        "bot": "d14808a8.d291e8",
        "x": 660,
        "y": 1000,
        "wires": [
            [
                "5d781b2a.ee18b4",
                "b0958ae4.c8b248"
            ]
        ]
    },
    {
        "id": "5d781b2a.ee18b4",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "Telegram Sender - reply",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "x": 870,
        "y": 1040,
        "wires": []
    },
    {
        "id": "b0958ae4.c8b248",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "Merge reply with saved cmd",
        "func": "const pendingCmds = flow.get('pendingCmds')\n\nmsg.payload.content = `${pendingCmds[ msg.originalMessage.reply_to_message.message_id ]} ${msg.payload.content}`\n\ndelete pendingCmds[ msg.originalMessage.reply_to_message.message_id ]\n\nflow.set('pendingCmds', pendingCmds)\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 880,
        "y": 1000,
        "wires": [
            [
                "8cda46f2.53e0d8"
            ]
        ]
    },
    {
        "id": "6edcd4e6.4e2abc",
        "type": "link out",
        "z": "25a82b05.7c01a4",
        "name": "Ask for more info (out)",
        "links": [
            "10705c90.dd1323"
        ],
        "x": 915,
        "y": 720,
        "wires": []
    },
    {
        "id": "10705c90.dd1323",
        "type": "link in",
        "z": "25a82b05.7c01a4",
        "name": "Ask for more info (in)",
        "links": [
            "6edcd4e6.4e2abc"
        ],
        "x": 160,
        "y": 1000,
        "wires": [
            [
                "d1a04273.367c9"
            ]
        ]
    },
    {
        "id": "8cda46f2.53e0d8",
        "type": "link out",
        "z": "25a82b05.7c01a4",
        "name": "Re-process (out)",
        "links": [
            "c09758a6.8a0f08"
        ],
        "x": 1075,
        "y": 1000,
        "wires": []
    },
    {
        "id": "c09758a6.8a0f08",
        "type": "link in",
        "z": "25a82b05.7c01a4",
        "name": "Re-process (in)",
        "links": [
            "8cda46f2.53e0d8",
            "742e7111.056c6"
        ],
        "x": 155,
        "y": 640,
        "wires": [
            [
                "5cbce84f.0c7038"
            ]
        ]
    },
    {
        "id": "6ec2cae1.c7fb34",
        "type": "telegram command",
        "z": "25a82b05.7c01a4",
        "name": "",
        "command": "/on",
        "bot": "d14808a8.d291e8",
        "x": 130,
        "y": 860,
        "wires": [
            [
                "d1567e12.950e"
            ],
            []
        ],
        "outputLabels": [
            "Authorised + Matches Command",
            "Authorised - No command match"
        ]
    },
    {
        "id": "cf3dcb90.3e6f98",
        "type": "telegram command",
        "z": "25a82b05.7c01a4",
        "name": "",
        "command": "/off",
        "bot": "d14808a8.d291e8",
        "x": 130,
        "y": 920,
        "wires": [
            [
                "d1567e12.950e"
            ],
            []
        ],
        "outputLabels": [
            "Authorised + Matches Command",
            "Authorised - No command match"
        ]
    },
    {
        "id": "d1567e12.950e",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "Translate Input to /switch cmd",
        "func": "// Turn /on or /off into a /light command & send through its processing\n\nconst cmd = msg.originalMessage.text.substr(0,3).toLowerCase() === '/on' ? 'on' : 'off'\n\nmsg.originalMessage.text = `/switch ${cmd} ${msg.payload.content.trim()}`\n\nmsg.payload.content = `${cmd} ${msg.payload.content.trim()}`\n\nreturn msg\n",
        "outputs": 1,
        "noerr": 0,
        "x": 370,
        "y": 880,
        "wires": [
            [
                "742e7111.056c6"
            ]
        ]
    },
    {
        "id": "742e7111.056c6",
        "type": "link out",
        "z": "25a82b05.7c01a4",
        "name": "Re-process (out)",
        "links": [
            "c09758a6.8a0f08"
        ],
        "x": 555,
        "y": 880,
        "wires": []
    },
    {
        "id": "58f289f8.8bbed8",
        "type": "telegram command",
        "z": "25a82b05.7c01a4",
        "name": "",
        "command": "/h",
        "bot": "d14808a8.d291e8",
        "x": 130,
        "y": 540,
        "wires": [
            [
                "472a916f.19feb"
            ],
            []
        ],
        "outputLabels": [
            "Authorised + Matches Command",
            "Authorised - No command match"
        ]
    },
    {
        "id": "236ea675.41ed8a",
        "type": "inject",
        "z": "25a82b05.7c01a4",
        "name": "",
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "repeat": "",
        "crontab": "",
        "once": true,
        "onceDelay": 0.1,
        "x": 130,
        "y": 40,
        "wires": [
            [
                "d797c17b.12e8d"
            ]
        ]
    },
    {
        "id": "d797c17b.12e8d",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "Save Switch MetaData to Global switchLocations",
        "func": "// Record the physical locations of logical switch ID's\n// A switch recieves commands (a device sends data)\nglobal.set('switchLocations', {\n  \"SWITCH01\" : {location: \"HOME/IN/00/LIVING\",   description: \"Living room\",                      type: \"Siemens white remote plug\"},\n  \"SWITCH02\" : {location: \"HOME/IN/00/HALL\",     description: \"Hall (Rear)\",                      type: \"Siemens white remote plug\"},\n  \"SWITCH03\" : {location: \"HOME/IN/00/ENTRANCE\", description: \"Hall (Front)\",                     type: \"Siemens white remote plug\"},\n  \"SWITCH04\" : {location: \"HOME/IN/01/LANDING\",  description: \"Landing light\",                    type: \"Nexa remote plug\"},\n  \"SWITCH05\" : {location: \"HOME/OUT/00/TREE\",    description: \"Tree lights\",                      type: \"Nexa remote plug\"},\n  \"SWITCH06\" : {location: \"HOME/IN/02/LOFT\",     description: \"Loft LED lights\",                  type: \"Siemens white remote plug\"},\n  \"SWITCH07\" : {location: \"HOME/IN/99/NA\",       description: \"Not in use\",                       type: \"Siemens white remote plug\"},\n  \"SWITCH08\" : {location: \"HOME/IN/99/NA\",       description: \"Not in use\",                       type: \"Siemens white remote plug\"},\n  \"SWITCH09\" : {location: \"HOME/IN/00/ROAMING\",  description: \"Edimax\",                           type: \"Siemens remote/Edimax Smartswitch SP01\"},\n  \"BELL01\"   : {location: \"HOME/IN/00/HALL\",     description: \"Hall bell sounder near kitchen\",   type: \"Nexa bell sounder\"},\n});\n",
        "outputs": 1,
        "noerr": 0,
        "x": 450,
        "y": 40,
        "wires": [
            []
        ]
    },
    {
        "id": "ed71fe79.4b54c",
        "type": "function",
        "z": "25a82b05.7c01a4",
        "name": "Set Telegram message options",
        "func": "//{\"chatId\":202430638, \"type\":\"message\", \"text\":\"This is some text\"}\n//msg.payload.options = {parse_mode : \"Markdown\"}; // or HTML\n//// NB: The Family Knight group is connected to the IFTTT bot\n\nconst isObject = function (obj) {\n    // Lots of alternatives here: https://stackoverflow.com/questions/8511281/check-if-a-value-is-an-object-in-javascript\n    return Object.prototype.toString.call(obj) === '[object Object]';\n}\n\nif ( msg.topic === '' ) msg.topic = 'Node-RED Bot'\n\nconst payload = msg.payload\n\nif ( ! isObject(msg.payload) ) {\n    msg.payload = {}\n}\n\nmsg.payload.type = 'message'\nif ( ! msg.payload.options ) msg.payload.options = {}\n\nif ( !msg.payload.chatId && msg.chatId ) msg.payload.chatId = msg.chatId\nif ( !msg.payload.parse_mode && msg.parse_mode ) msg.payload.options.parse_mode = msg.parse_mode\n    \nif ( msg.replyTo ) msg.payload.options.reply_to_message_id = msg.replyTo\n\nmsg.payload.content = msg.topic + '\\n' + payload\n\nreturn msg;\n",
        "outputs": "1",
        "noerr": 0,
        "x": 530,
        "y": 120,
        "wires": [
            [
                "e1123e72.f9af",
                "5a4d3edc.28e11"
            ]
        ],
        "outputLabels": [
            "New Msg (for Telegram)"
        ]
    },
    {
        "id": "e1123e72.f9af",
        "type": "telegram sender",
        "z": "25a82b05.7c01a4",
        "name": "",
        "bot": "d14808a8.d291e8",
        "x": 790,
        "y": 120,
        "wires": [
            [
                "820d9418.f59fe8"
            ]
        ]
    },
    {
        "id": "ccde84c1.79ce58",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "Telegram Sender ERROR",
        "active": true,
        "console": "false",
        "complete": "true",
        "x": 1150,
        "y": 100,
        "wires": []
    },
    {
        "id": "5a4d3edc.28e11",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "",
        "active": true,
        "console": "false",
        "complete": "true",
        "x": 750,
        "y": 80,
        "wires": []
    },
    {
        "id": "bb2d9017.65d8f",
        "type": "link in",
        "z": "25a82b05.7c01a4",
        "name": "Telegram Out to JkPi2 Bot",
        "links": [
            "3029e7bd.5ebb18",
            "5bb2a00e.7c072",
            "b3711092.3c84a",
            "b86d7c2c.8e92a",
            "d8a95928.1c08c8",
            "f42fbd8.e58ed4",
            "68a6bd32.cdeac4",
            "dbc61113.e382b",
            "bb0c2efb.d8d75",
            "e8492903.2dea48"
        ],
        "x": 58,
        "y": 100,
        "wires": [
            [
                "9ea10b41.3ab0c8"
            ]
        ]
    },
    {
        "id": "535d5c07.6013a4",
        "type": "link in",
        "z": "25a82b05.7c01a4",
        "name": "Telegram Out to Family Knight Group",
        "links": [
            "13c26be0.0e8fe4",
            "5bb2a00e.7c072",
            "68a6bd32.cdeac4"
        ],
        "x": 58,
        "y": 140,
        "wires": [
            [
                "72d43eb9.54445"
            ]
        ]
    },
    {
        "id": "820d9418.f59fe8",
        "type": "switch",
        "z": "25a82b05.7c01a4",
        "name": "",
        "property": "error",
        "propertyType": "msg",
        "rules": [
            {
                "t": "nnull"
            },
            {
                "t": "else"
            }
        ],
        "checkall": "true",
        "outputs": 2,
        "x": 950,
        "y": 120,
        "wires": [
            [
                "ccde84c1.79ce58"
            ],
            [
                "bf7a471d.4dff78"
            ]
        ],
        "outputLabels": [
            "Error",
            null
        ]
    },
    {
        "id": "bf7a471d.4dff78",
        "type": "debug",
        "z": "25a82b05.7c01a4",
        "name": "Telegram Sender",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 1130,
        "y": 140,
        "wires": []
    },
    {
        "id": "9ea10b41.3ab0c8",
        "type": "change",
        "z": "25a82b05.7c01a4",
        "name": "chatId: JkPi2 Bot",
        "rules": [
            {
                "t": "set",
                "p": "chatId",
                "pt": "msg",
                "to": "202430638",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 190,
        "y": 100,
        "wires": [
            [
                "ed71fe79.4b54c"
            ]
        ]
    },
    {
        "id": "72d43eb9.54445",
        "type": "change",
        "z": "25a82b05.7c01a4",
        "name": "chatId:Family Knight Group",
        "rules": [
            {
                "t": "set",
                "p": "chatId",
                "pt": "msg",
                "to": "-149471560",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 220,
        "y": 140,
        "wires": [
            [
                "ed71fe79.4b54c"
            ]
        ]
    },
    {
        "id": "1a1b7cff.9511b3",
        "type": "mqtt-broker",
        "z": "",
        "name": "",
        "broker": "192.168.1.167",
        "port": "1883",
        "clientid": "",
        "usetls": false,
        "compatmode": false,
        "keepalive": "60",
        "cleansession": true,
        "willTopic": "",
        "willQos": "0",
        "willPayload": "",
        "birthTopic": "",
        "birthQos": "0",
        "birthPayload": ""
    },
    {
        "id": "d14808a8.d291e8",
        "type": "telegram bot",
        "z": "",
        "botname": "JkPi2",
        "usernames": "",
        "chatids": ""
    }
]
```
{% endraw %}
