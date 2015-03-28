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

## Lab 2.10: Building an API server
To list and save tasks. And yay! Finally done with the Hello World!

Create a `tasks` directory and go into it:
```
$ mkdir tasks && cd tasks
```

Initialize the project with `npm init`
```
$ npm init
```

Import the `express`, `express-session`, and `body-parser` packages into your project:
```
$ npm install --save express express-session body-parser
```

Finally, create an empty `index.js` file:
```
$ touch index.js
```

**index.js**
```javascript
var express = require('express');
var session = require('express-session');
var bodyParser = require('body-parser');
var app = express();

app.use(session({secret: 'ntu-ieee'}));
app.use(bodyParser.urlencoded({ extended: false }));

var tasks = [];
tasks.push('Step 1: Learn Node');
tasks.push('Step 2: Learn NPM');
tasks.push('Step 3: Learn Express');

app.get('/', function (req, res) {
	if (!req.session.tasks) {
		console.log('No session');
		req.session.tasks = tasks;
	} else {
		console.log('Session found');
	}
	res.json({tasks: req.session.tasks});
});

app.post('/task', function (req, res) {
	var _tasks = [];
	if (!req.session.tasks) {
		_tasks = tasks;
	} else {
		console.log('Session found');
		_tasks = req.session.tasks;
	}
	console.log(req.body);
	_tasks.push(req.body.task);
	req.session.tasks = _tasks;
	res.json(req.session.tasks);
});

app.listen(3000, function () {
	console.log('API server started on port 3000');
});
```
