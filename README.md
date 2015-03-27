# Node.js
Lab code for the Node.js workshop

## Lab 2.1: Test the waters via REPL
```
$ node

> var a = [1, 2, 3];

> console.log(a);
[ 1, 2, 3 ]

> a.forEach(function (z) { console.log(z); });
1
2
3
```
You can exit the REPL using typing `.exit` or pressing `Ctrl` + `D`

## Lab 2.2: Baby-steps
Building the classic Hello World

**index.js**
```javascript
console.log('Hello NTU');
```

## Lab 2.3: Requiring things
Modifying your previous Hello World example

**greet.js**
```javascript
exports.hello = function () {
	return 'Hello NTU';
}
```
**index.js**
```javascript
var greet = require('./greet.js');
console.log(greet.hello());
```

## Lab 2.4: Requiring things
Let’s get bilingual

**greet.js**
```javascript
exports.hello = function () {
	return 'Hello NTU';
}
exports.konbanwa = function () {
	return 'Konbanwa NTU';
}
```
**index.js**
```javascript
var greet = require('./greet.js');
console.log(greet.hello());
console.log(greet.konbanwa());
```

## Lab 2.5: Requiring things
Another way of handling exports

**greet.js**
```javascript
module.exports = {
	hello: function () {
		return 'Hello NTU';
	},
	konbanwa: function () {
		return 'Konbanwa NTU';
	}
}
```

## Lab 2.6: Create a better (Hello) World
By building a web server

**index.js**
```javascript
var http = require('http');
var greet = require('./greet.js');

http.createServer(function (req, res) {
	res.writeHead(200, {'Content-Type': 'text/plain'});
	res.end(greet.hello());
}).listen(8000);

console.log('Server running at http://127.0.0.1:8000');
```

## Lab 2.7: Initializing your Hello World project
It's just `npm init`... easy peasy :)

## Lab 2.8: Saving the moment
Installing and using a 3rd party module

```
$ npm install --save moment
```

**index.js**
```javascript
var http = require('http');
var greet = require('./greet.js');
var moment = require('moment');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hi! It is now ' + moment().format('h:mm:ss a'));
}).listen(8000);

console.log('Server running at http://127.0.0.1:8000');
```

## Lab 2.9: Expressive greetings
Modify your Hello World code to use Express

```
$ npm install --save express
```

**index.js**
```javascript
var express = require('express');
var greet = require('./greet.js');
var app = express();

app.get('/', function (req, res) {
	res.send(greet.hello());
});

app.listen(8000);

console.log('Server running at http://127.0.0.1:8000');
```
