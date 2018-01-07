---
title: Making Node-RED available over the Internet
description: >
    There are already too many people putting up insecure services over the Internet. Please don't add to this problem.

    This article explains a simple way to get some level of connectivity to Node-RED over the Internet without security issues.

    It also explains some of the wider issues and where to go for more details.
comments: true
date: 2018-01-07 10:00:00
---

The simplest approach to making Node-RED available over the Internet is not to do it at all!
Node-RED was not initially designed to be exposed direct to the Internet but rather as a prototyping tool for IoT and Home Automation. As such, it does not have regular security audits and you should very much limit its exposure.

## The next simplest approach: A Telegram bot

However, access over the Internet is a common requirement and certainly extends the usefulness of Node-RED.

So the next simplest approach is to make use of a secure 3rd-party messaging service such as [Telegram Messenger](https://telegram.org/). Telegram has client apps for just about every platform. It is fast, robust, secure and has a relatively easy to use [API](https://core.telegram.org/api) (_[Application Programming Interface](https://en.wikipedia.org/wiki/Application_programming_interface)_).

Create a [bot](https://en.wikipedia.org/w/index.php?title=Automated_bot&redirect=no) in Telegram, grab the API connection details and install the [node-red-contrib-telegrambot](https://flows.nodered.org/node/node-red-contrib-telegrambot) nodes into Node-RED.

You can use the bot to send information to Telegram clients and also to allow authorised clients/users/groups to send information and commands to Node-RED via the bot.

The main current limitation is that you cannot use your bot to bridge information from other Telegram bots into Node-RED.

Also, using an instant messaging style interface does limit how you can interact with your users. However, telegram allows a number of ways ranging from conversational interfaces to buttons & autocomplete.

You can also use a bot to send rich data including video and audio to clients. You can also receive such data and other files from users.

If you are trying to create a conversational interface, you might find [RiveScript](https://www.rivescript.com/) to be useful. There are two nodes available for Node-RED, [node-red-contrib-rive](https://flows.nodered.org/node/node-red-contrib-rive) and [node-red-contrib-rivescript](https://flows.nodered.org/node/node-red-contrib-rivescript).

## Alternative approaches

If a Telegram bot doesn't meet your requirements, you will want to ensure that all Node-RED web interfaces (including admin, Dashboard, http-in/out, etc) are all properly protected. You will also need to protect the websockets and Socket.IO interfaces that are used. This is non-trivial and care is needed when configuring.

Some additional information is available on the [WIKI for the Node-RED Cookbook](https://github.com/node-red/cookbook.nodered.org/wiki/How-to-safely-expose-Node-RED-to-the-Internet).

Generally, the safer approach is to put Node-RED behind a [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy). [NGINX](https://www.nginx.com/resources/admin-guide/reverse-proxy/) or [HAProxy](http://www.haproxy.org/) are two commonly used applications for this purpose.
