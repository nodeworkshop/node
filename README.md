# node
Lab code for the Node.js workshop

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
