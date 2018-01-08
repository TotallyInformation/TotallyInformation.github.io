---
title: Node-RED Dashboard Template Examples (AngularJS)
description: >
    Some examples of using AngularJS with the Node-RED Dashboard.

    Not all features of Angular are accessible via the Dashboard.
comments: true
date: 2018-01-08 22:00:00
---

## Accessing the msg object

When using Dashboard, the Node-RED server (the back end) sends data via Socket.IO to the client browser (the front end). This comes in the form of a `msg` object. in similar form to the msg that flows through the rest of Node-RED.

To access the `msg` object from within a client Angular script. Put this into a Dashboard Template node:

```javascript
<div ng-bind-html="msg.payload"></div>
<script>
    //console.dir(scope) // this also works
    //console.dir(scope.msg) // This doesn't because scope.msg doesn't yet exist

    // Lambda function to access the Angular Scope
    ;(function(scope) {
        //console.log('--- SCOPE ---')
        //console.dir(scope) // this works but you only get it once (on startup)
        //console.dir(scope.msg) // Doesn't work for  because scope.msg doesn't yet exist

        //Have to use $watch so we pick up new, incoming msg's
        scope.$watch('msg.payload', function(newVal, oldVal) {
            console.log('- Scope.msg -')
            console.dir(scope.msg)
        })

    })(scope)
</script>
```

## A couple of ways to send a msg back to Node-RED

This shows you how to set up some buttons and a slider widget that send data back to Node-RED. As always, add this code to a Dashboard Template node.

```javascript
<div>{{msg.payload}}</div>

<md-button ng-click="send({payload: 'Hello World'})">
    Click me to send a hello world
</md-button>

<md-slider ng-model="msg.payload" ng-change="send(msg)"></md-slider>

<div flex layout="row" layout-align="space-around center">
    <md-button ng-repeat="b in buttons" class="md-icon-button" ng-click="click(b)">
        <ng-md-icon icon="{{msg.payload[b.payload]?b.icon2:b.icon}}"
                    ng-style="{color: msg.payload[b.payload]?b.color2:b.color}"></ng-md-icon>
    </md-button>
</div>

<script>
    scope.buttons = [{
        icon: 'pause', color: 'black',
        icon2: 'play_arrow', color2: 'red',
        payload: 'play',
    }, {
        icon: 'alarm', color: 'black',
        icon2: 'alarm', color2: 'red',
        payload: 'alarm',
    }];

    scope.click = function(b) {
        if (!this.msg) this.msg = {};
        if (!this.msg.payload) this.msg.payload = {};
        this.msg.payload[b.payload] = !this.msg.payload[b.payload];
        this.send(this.msg);
    }.bind(scope);
</script>
```

## Date Picker Example

```javascript
<div>
   <md-datepicker ng-model="myDate" md-placeholder="Enter date" ng-change="send({payload: myDate})"></md-datepicker>
</div>
```

## Dynamically inject HTML

Use this code in a Dashboard Template to render a msg.payload containing HMTL.

```javascript
<div ng-bind-html="msg.payload"></div>
```

## Change the Angular delimiters

If you want to send data through a Node-RED Template node before sending it to a Dashboard Template node, you will find that, since both use the same delimiters (double curly brackets). So you need to change the delimiters in the Dashboard Template.

```javascript
<!DOCTYPE html>
<html ng-app="ngApp">
<head></head>
<body ng-controller="ngCtrl">

    <h1>Topic: {[{topic}]}</h1>
    <pre>
{[{payload | json }]}
    </pre>


    <script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.4.12/angular.min.js"></script>
    <script>
        var ngApp = angular.module('ngApp', []);

        ngApp.config(function($interpolateProvider) {
            $interpolateProvider.startSymbol('{[{');
            $interpolateProvider.endSymbol('}]}');
        });

        ngApp.controller('ngCtrl', ['$scope', function($scope) {
            // Need to wrap with TRY in case not valid JSON
            $scope.payload = JSON.parse(decodeEntities('{{payload}}'));
            $scope.topic = '{{topic}}';
        }]);

        function decodeEntities(encodedString) {
            var textArea = document.createElement('textarea');
            textArea.innerHTML = encodedString;
            return textArea.value;
        }
    </script>
</body>
</html>
```
