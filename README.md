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
