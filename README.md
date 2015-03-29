# Node.js
The following labs are part of a Node.js workshop held at Nanyang Technological Unviversity, Singapore on 27th March 2015.

Accompanying slides are available at: http://www.slideshare.net/quhan/nodejs-workshop-46416648

Please take a look at the [Setup](https://github.com/nodeworkshop/setup) instructions if you haven't installed Node.js on your machine.

## Lab 2.1: Test the waters via REPL
#### The Read–Eval–Print Loop
Start up the REPL by typing `node` in your command line:
```
$ node
```

Then try out these Javascript commands:
```
> var a = [1, 2, 3];

> console.log(a);
[ 1, 2, 3 ]

> a.forEach(function (z) { console.log(z); });
1
2
3
```
Please disregard the `undefined` statements you see when you type in each command. For more info, check out this [stackoverflow post](http://stackoverflow.com/a/8457476).

You can exit the REPL using typing `.exit` or pressing `Ctrl` + `D`.

## Lab 2.2: Baby-steps
#### Building the classic Hello World

Create a `hello` folder and go into it:
```
$ mkdir hello && cd hello
```

Next, create an empty `index.js` file in that folder:
```
$ touch index.js
```

Type the following line into the `index.js` file:

**index.js**
```javascript
console.log('Hello NTU');
```

Finally, save and exit the file. Once at the command line, execute your application by running:
```
$ node index.js
```

## Lab 2.3: Requiring things
#### Modifying your previous Hello World example

In that `hello` folder, create a new file called `greet.js`:
```
$ touch greet.js
```

Type the following lines into the `greet.js` file:

**greet.js**
```javascript
exports.hello = function () {
	return 'Hello NTU';
}
```
Next, open up your `index.js` file and modify it so it looks like:

**index.js**
```javascript
var greet = require('./greet.js');
console.log(greet.hello());
```

Finally, execute your application by:
```
$ node index.js
```

## Lab 2.4: Requiring things (again)
#### Let’s get bilingual

Modify the `greet.js` and `index.js` files as follows:

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

Hopefully by now, you should already be familiar with how to run your application. In case you don't, here's the command one last time:
```
$ node index.js
```

## Lab 2.5: Requiring things (one last time)
#### Another way of handling exports

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

## Lab 2.6: Creating a better (Hello) World
#### By building a web server

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

This time around, after you've run `node index.js`, launch your favorite web browser and go to `http://127.0.0.1:8000`.

Congratulations! You've just built your very own web server! :thumbsup:

Remember to quit your Node application by pressing `Ctrl`+`C`.

## Lab 2.7: Initializing your Hello World project
#### With metadata

Easy peasy... just type the following in your `hello` folder:

```
$ npm init
```

Once it's done, `npm init` will generate a `package.json` file in your folder. This file contains metadata for your application.

## Lab 2.8: Saving the moment
#### Installing and using a 3rd party module

Run the following command to import this 3rd party module from the NPM registry over to your app:
```
$ npm install --save moment
```

Note that the `--save` in the command will add this as a **dependency** to your app's `package.json` file.

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

Just like lab 2.6 (and all labs following this), run your application by typing `node index.js` and go to `http://127.0.0.1:8000` using a web browser.

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
To list and save tasks. And yay! Finally done with the Hello World! :relieved:

Create a `tasks` folder and go into it:
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

// Middleware for handling session/cookies
app.use(session({secret: 'ntu-ieee'}));

// Middleware for handling the POST requests
app.use(bodyParser.urlencoded({extended: false}));

// In real life, this is usually some form of DB call.
function initializeTasks() {
	var tasks = [];
	tasks.push('Step 1: Learn Node');
	tasks.push('Step 2: Learn NPM');
	tasks.push('Step 3: Learn Express');
	return tasks;
}

app.get('/', function (req, res) {
	if (!req.session.tasks) {
		// Tasks not found in session, so initialize it with an array of tasks
		req.session.tasks = initializeTasks();
	}

	// Returns a JSON object with an array of tasks
	res.json({tasks: req.session.tasks});
});

app.post('/task', function (req, res) {
	if (!req.session.tasks) {
		// Tasks not found in session, so initialize it with an array of tasks
		req.session.tasks = initializeTasks();
	}

	// Assign the POSTed task to the newTask variable
	var newTask = req.body.task;
	
	// Save the new task to the session array of tasks
	req.session.tasks.push(newTask);

	// Returns a JSON object with an array of tasks
	res.json({tasks: req.session.tasks});
});

app.listen(3000, function () {
	console.log('API server started on port 3000');
});
```

This time around, after `node index.js`, you'll need to access your API server on port `3000`.

While you can view the list of tasks via your browser (`http://localhost:3000`), it's best to use an application like [Postman](https://www.getpostman.com/features) to fully test both the **GET** and **POST** functions of your app.
