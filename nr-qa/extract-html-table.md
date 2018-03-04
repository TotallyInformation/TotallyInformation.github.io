---
title: 'How to extract a table from HTML'
description: 'While Node-RED has some nodes for extracting data from HTML, the nodes are rather simplistic. Here is a quick way to extract the data from an HTML table element.'
comments: true
date: 2018-01-05 15:27:00
updated: 2018-03-04 14:00:00
---

While Node-RED has some nodes for extracting data from HTML, the nodes are rather simplistic.

Here is a quick way to extract the data from an HTML table element.

To use it, you need to install the [cheerio](https://www.npmjs.com/package/cheerio) node.js module to your user data folder (usually `~/.node-red`).

Then you need to reference it in the global variables part of your `settings.js` file (same location as above).

{% raw %}
```javascript
    functionGlobalContext: {
        cheerio: require('cheerio'),
    },
```
{% endraw %}

Then add a flow that returns html from a web page (using the Request node).

Then add a function node with the following code:

{% raw %}
```javascript
/*jshint sub:true,asi:true,maxerr:1000*/

const tableSelector = '#ContentPlaceHolder1_grdStation'

const cheerio = global.get('cheerio')
const $ = cheerio.load(msg.payload)
const options = {
    rowForHeadings: 0,  // extract th cells from this row for column headings (zero-based)
    ignoreHeadingRow: true, // Don't tread the heading row as data
    ignoreRows: [],
}
const jsonReponse = []
const columnHeadings = []

$(tableSelector).each(function(i, table) {
    var trs = $(table).find('tr')

    // Set up the column heading names
    getColHeadings( $(trs[options.rowForHeadings]) )

    // Process rows for data
    $(table).find('tr').each(processRow)
})

msg.payload = {
    columnHeadings: columnHeadings,
    rows: jsonReponse,
}

return msg

function getColHeadings(headingRow) {
    const alreadySeen = {}

    $(headingRow).find('th').each(function(j, cell) {
        let tr = $(cell).text().trim()

        if ( alreadySeen[tr] ) {
            let suffix = ++alreadySeen[tr]
            tr = `${tr}_${suffix}`
        } else {
            alreadySeen[tr] = 1
        }

        columnHeadings.push(tr)
    })
}

function processRow(i, row) {
    const rowJson = {}

    if ( options.ignoreHeadingRow && i === options.rowForHeadings ) return
    // TODO: Process options.ignoreRows

    $(row).find('td').each(function(j, cell) {
        rowJson[ columnHeadings[j] ] = $(cell).text().trim()
    })

    // Skip blank rows
    if (JSON.stringify(rowJson) !== '{}') jsonReponse.push(rowJson)
}

//EOF
```
{% endraw %}

The result is a `msg.payload` containing 2 arrays. The first is the list of column names.
The second is an array of objects, each object containing a property for each column.

Code inspired by: https://github.com/iaincollins/tabletojson/blob/master/lib/tabletojson.js
and by a [conversation in the Node-RED Google Group](https://groups.google.com/d/topic/node-red/J0020rPetAY/discussion).

## Example Flow

{% raw %}
```
[{"id":"815bf28f.d0691","type":"inject","z":"e7463dd2.db517","name":"get data","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":120,"y":100,"wires":[["46f8420f.a312dc"]]},{"id":"46f8420f.a312dc","type":"http request","z":"e7463dd2.db517","name":"","method":"GET","ret":"txt","url":"http://publicinfobanjir.water.gov.my/View/OnlineFloodInfo/PublicWaterLevel.aspx?scode=SEL","tls":"","x":330,"y":100,"wires":[["808d9c98.e305d"]]},{"id":"fe447848.e25938","type":"debug","z":"e7463dd2.db517","name":"","active":true,"complete":false,"x":810,"y":60,"wires":[]},{"id":"d0660df5.4f339","type":"function","z":"e7463dd2.db517","name":"","func":"/*jshint sub:true,asi:true,maxerr:1000*/\n\nconst tableSelector = '#ContentPlaceHolder1_grdStation'\n\nconst cheerio = global.get('cheerio')\nconst $ = cheerio.load(msg.payload)\nconst options = {\n    rowForHeadings: 0,  // extract th cells from this row for column headings (zero-based)\n    ignoreHeadingRow: true, // Don't tread the heading row as data\n    ignoreRows: [],\n}\nconst jsonReponse = []\nconst columnHeadings = []\n\n$(tableSelector).each(function(i, table) {\n    var trs = $(table).find('tr')\n    \n    // Set up the column heading names\n    getColHeadings( $(trs[options.rowForHeadings]) )\n\n    // Process rows for data\n    $(table).find('tr').each(processRow)\n})\n\nmsg.payload = {\n    columnHeadings: columnHeadings,\n    rows: jsonReponse,\n}\n\nreturn msg\n\nfunction getColHeadings(headingRow) {\n    const alreadySeen = {}\n    \n    $(headingRow).find('th').each(function(j, cell) {\n        let tr = $(cell).text().trim()\n        \n        if ( alreadySeen[tr] ) {\n            let suffix = ++alreadySeen[tr]\n            tr = `${tr}_${suffix}`\n        } else {\n            alreadySeen[tr] = 1\n        }\n        \n        columnHeadings.push(tr)\n    })\n}\n\nfunction processRow(i, row) {\n    const rowJson = {}\n    \n    if ( options.ignoreHeadingRow && i === options.rowForHeadings ) return\n    // TODO: Process options.ignoreRows\n    \n    $(row).find('td').each(function(j, cell) {\n        rowJson[ columnHeadings[j] ] = $(cell).text().trim()\n    })\n    \n    // Skip blank rows\n    if (JSON.stringify(rowJson) !== '{}') jsonReponse.push(rowJson)\n}\n\n//EOF","outputs":"1","noerr":0,"x":650,"y":100,"wires":[["fe447848.e25938","c35dcf95.5cc9","641996d3.fe8118"]]},{"id":"641996d3.fe8118","type":"change","z":"e7463dd2.db517","name":"","rules":[{"t":"set","p":"selected","pt":"msg","to":"{}","tot":"json"},{"t":"set","p":"selected.station","pt":"msg","to":"payload.rows[40]['Station ID']","tot":"msg"},{"t":"set","p":"selected.level","pt":"msg","to":"payload.rows[40]['River Level (m)']","tot":"msg"},{"t":"move","p":"selected","pt":"msg","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":820,"y":100,"wires":[["f0152260.70e1d"]]},{"id":"f0152260.70e1d","type":"debug","z":"e7463dd2.db517","name":"","active":true,"complete":false,"x":990,"y":100,"wires":[]},{"id":"c35dcf95.5cc9","type":"change","z":"e7463dd2.db517","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"$$.payload.rows.(\t   {\t       \"station\": $.'Station ID',\t       \"name\": $.'Station Name',\t       \"level\": $.'River Level (m)',\t       \"alert\": $number($.'River Level (m)') > $number($.Alert) ? true : false\t   }\t)","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":820,"y":140,"wires":[["d505285a.26bcb8"]]},{"id":"d505285a.26bcb8","type":"debug","z":"e7463dd2.db517","name":"","active":true,"complete":false,"x":990,"y":140,"wires":[]},{"id":"808d9c98.e305d","type":"function","z":"e7463dd2.db517","name":"cache","func":"/*jshint sub:true,asi:true,maxerr:1000*/\n// Expects input msgs with topic set \n\nconst contextVarName = 'httpReqMsgs' // homeMsgs\n\n// saved context\nvar cachedMsgs = context.get(contextVarName) || {}\n\n// Only send to single client if needed\nvar socketId = null\nif ( msg.hasOwnProperty('_socketId') ) {\n    socketId = msg._socketId\n}\n\n// Replay cache if requested\nif ( msg.hasOwnProperty('cacheControl') && msg.cacheControl.toUpperCase() === 'REPLAY' ) {\n    for (var topic in cachedMsgs) {\n        let newMsg = {\n            \"topic\": topic, \n            \"payload\": cachedMsgs[topic]\n        }\n        // Only send to a single client if we can\n        if ( socketId !== null ) newMsg._socketId = socketId\n        node.send(newMsg)\n    }\n    return null\n}\n// -- else if --\n// Empty cache if requested\nif ( (msg.hasOwnProperty('cacheControl') && msg.cacheControl === 'RESET')  ||\n     (msg.payload.hasOwnProperty('cacheControl') && msg.payload.cacheControl === 'RESET') ) {\n    cachedMsgs = {}\n    context.set(contextVarName, cachedMsgs)\n    return null\n}\n// -- else --\n\n// ignore cacheControl and uibuilder control messages\nif ( msg.hasOwnProperty('cacheControl') || msg.hasOwnProperty('uibuilderCtrl') ) return null\n\n// Add a counter for each device name\nif ( msg.topic.endsWith('$name') ) {\n    let topic = msg.topic.replace('$name', '$count')\n    let count = cachedMsgs[topic] || 0\n    count = count + 1\n    cachedMsgs[topic] = count\n    let newMsg = {\n        \"topic\": topic, \n        \"payload\": count\n    }\n    // Only send to a single client if we can\n    if ( socketId !== null ) newMsg._socketId = socketId\n    node.send(newMsg)\n}\n\n// Keep the last msg.payload by topic\ncachedMsgs[msg.topic] = msg.payload\n\n// save context for next time\ncontext.set(contextVarName, cachedMsgs)\n\nreturn msg;","outputs":1,"noerr":0,"x":510,"y":100,"wires":[["d0660df5.4f339","c0cacccb.9e3c7"]]},{"id":"afc218de.706f88","type":"inject","z":"e7463dd2.db517","name":"replay","topic":"","payload":"","payloadType":"str","repeat":"","crontab":"","once":false,"x":110,"y":160,"wires":[["eddcc57e.277418"]]},{"id":"c0cacccb.9e3c7","type":"debug","z":"e7463dd2.db517","name":"","active":false,"complete":"true","x":650,"y":180,"wires":[]},{"id":"eddcc57e.277418","type":"change","z":"e7463dd2.db517","name":"","rules":[{"t":"set","p":"cacheControl","pt":"msg","to":"REPLAY","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":300,"y":160,"wires":[["808d9c98.e305d"]]}]
```
{% endraw %}
