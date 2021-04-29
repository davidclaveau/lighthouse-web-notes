# Day 4 Notes

## Lecture Notes

### Functions as Values

* Anything that appears on the right side of an `=` sign is a value
* **Everything that's not a primitive is an object**
* This means that we can reassign a function to a different variable

```javascript
const sayHello = function(name) {
  console.log(`Hello there ${name}`)
};

const otherVariable = sayHello

otherVariable('Bob'); // Prints "Hello there Bob"
```

* You can *tecchnically* can assign a key-value pair to a function, but you would never need to.
  * Just goes to show that functions are just objects in JS

Can we use functions to call other functions? You bet!
```javascript
const runMyFunction = function(anotherFn) {
  console.log(anotherFn);
};

const sayHello = function(name) {
  console.log(`Hello there ${name}`)
};

runMyFunction(sayHello); // Prints [Function: sayHello]
```
* We can use `.toString()` in the `runMyFunction`

```javascript
const runMyFunction = function(anotherFn) {
  console.log(anotherFn.toString()); // Will print the function to the console as a string
};
```
* We refer to the function that we pass in as a **callback**
  * The initial function that takes the callback is considered a **higher-order function**
* Consider a higher order function as a function that takes in a function *instead* of other objects or primitive data
* A callback function is a function that gets passed to another function, to be invoked by that function

```javascript
const runMyFunction = function(callback) {
  callback('Bob');
};
```

Anonymous function

```javascript
const runMyFunction = function(callback) {
  callback('Bob');
};

runMyFunction(function(name) {
  console.log(`Something profound will happen to you ${name}`)
});

// Will print "Something profound will happen to you Bob"
```