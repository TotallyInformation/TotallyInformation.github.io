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

## Flow 1: Setup Tab

TBD

## Flow 2: Lighting

TBD
