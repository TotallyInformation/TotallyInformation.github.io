---
title: 'How to extract a table from HTML'
description: 'While Node-RED has some nodes for extracting data from HTML, the nodes are rather simplistic. Here is a quick way to extract the data from an HTML table element.'
comments: true
---

While Node-RED has some nodes for extracting data from HTML, the nodes are rather simplistic.

Here is a quick way to extract the data from an HTML table element.

To use it, you need to install `cheerio` to your user data folder (usually `~/.node-red`).

Then you need to reference it in the global variables part of your `settings.js` file (same location as above).

```javascript
    functionGlobalContext: {
        cheerio: require('cheerio'),
    },
```

Then add a flow that returns html from a web page (using the Request node).

Then add a function node with the following code:

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

The result is a `msg.payload` containing 2 arrays. The first is the list of column names.
The second is an array of objects, each object containing a property for each column.

Code inspired by: https://github.com/iaincollins/tabletojson/blob/master/lib/tabletojson.js
