# Functions and Recursion Intro

## Function as Values

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

* You can *technically* assign a key-value pair to a function, but you would never need to.

```js
sayHello.key = "value"

// But no one does this
```
* Just goes to show that functions are just objects in JS

* Can we use functions to call other functions? You bet!
```javascript
const runMyFunction = function(anotherFn) {
  console.log(anotherFn);
};

const sayHello = function(name) {
  console.log(`Hello there ${name}`)
};

runMyFunction(sayHello); // [Function: sayHello]
```
* We can use `.toString()` in the `runMyFunction`

```javascript
const runMyFunction = function(anotherFn) {
  console.log(anotherFn.toString()); // (Will print the function to the console as a string)
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

* Anonymous function

```javascript
const runMyFunction = function(callback) {
  callback('Bob');
};

runMyFunction(function(name) {
  console.log(`Something profound will happen to you ${name}`)
});

// When called: "Something profound will happen to you Bob"
```

### Arrow Functions

* Arrow functions were added as part of ES6 in 2015
  * ES === ECMA Script (European Computer Manufacturers Association)
  * Taken from CoffeeScript

```js
// Standard version
const sayHello = function(name) {
  console.log(`Hello there ${name}`);
};

// Arrow function
const sayHello = (name) => {
  console.log(`Hello there ${name}`);
};
```

* If there's a single line of code in the function, we can remove the curly braces and move it up

```js
const sayHello = (name) => console.log(`Hello there ${name}`);
```

* Arrow functions have implicit returns, so we don't need to write `return`

```js
const sayHello = (name) => `Hello there ${name}`;
```

* If there's only one parameter, you don't need the parenthesis

```js
const sayHello = name => `Hello there ${name}`;
```

* If there's no parameters, you still need `()` for the parameters

```js
const sayHello = () => `Hello there David`;
```

* Arrow functions do not create `this` and can't be used for it

### Our Own forEach Function

```js
const pets = ['Reggie', 'Amber' , 'Cookie', 'Arye', 'Charlie'];

pets.forEach(pet => console.log(`Do you want a scritch, ${pet}?`));

// Do you want a scritch, Reggie?
// Do you want a scritch, Amber?
// ...
```

* This can also be writter to be more modular, using a callback function with the `forEach` function

```js
const pets = ['Reggie', 'Amber' , 'Cookie', 'Arye', 'Charlie'];

const forEach = (arr, callback) => {
  for (const element of arr) {
    callback(element);
  }
};

forEach(pets, pet => console.log(`Do you want a scritch, ${pet}?`));

// Do you want a scritch, Reggie?
// Do you want a scritch, Amber?
// ...
```

* There are many other types of callback functions that we can create
  * `.map()`, .`filter()`, `.every()`, `.find()`, `.sort()`, `.reduce()`

* We can see how `.map()` works:
  * Provides a new array with the changes made to each element of that array
  * Don't forget that `.map()` creates a **new** array 

```js
const pets = ['Reggie', 'Amber' , 'Cookie', 'Arye', 'Charlie'];

const mapped = pets.map((pet) => {
  return `${pet} can has cheeseburger`;
});

console.log(mapped)

// [
//  'Reggie can has cheeseburger',
//  'Amber can has cheeseburger',
//  ...
// ]
```

* And we can create our own callback function of `.map()` like  so:

```js
const pets = ['Reggie', 'Amber' , 'Cookie', 'Arye', 'Charlie'];

const ourMap = (arr, callback) => {
  const returnArr = [];

  for (const element of arr) {
    const returnVal = callback(element);
    returnArr.push(returnVal);
  }

  return returnArr;
};

const mapper = ourMap(pets, (pet) => {
  return `Hey ${pet}! Quit scratching the couch!`
});

console.log(mapper);

// [
//  'Hey Reggie! Quit scratching the couch!',
//  'Hey Amber! Quit scratching the couch!',
//  ...
// ]
```

* We can see how `.filter()` works:
  * Creates a **new array** with the first value that matches the condition.

```js
const pets = ['Reggie', 'Amber' , 'Cookie', 'Arye', 'Charlie'];

const filtered = pets.filter((pet) => {
  if (pet === 'Reggie') {
    return true;
  }
  return false;

  // Or can be written as simply as this:
  // return pet === 'Reggie';
  // (example below)
});

console.log(filtered) // ['Reggie']
```

* This is the same as this:

```js
const pets = ['Reggie', 'Amber' , 'Cookie', 'Arye', 'Charlie'];

const filtered = pets.filter(pet => pet === 'Reggie');

console.log(filtered) // ['Reggie']
```

* And we can create our own callback function similar to `.filter()`

```js
const pets = ['Reggie', 'Amber' , 'Cookie', 'Arye', 'Charlie'];

const ourFilter = (arr, callback) => {
  const returnArr = [];

  for (const element of arr) {
    const returnVal = callback(element);
    
    if (returnVal) {
    returnArr.push(element);
    }
  }

  return returnArr;
};

const filtered = ourFilter(pets, (pet) => {
  return pet === 'Reggie';
});

console.log(filtered) // ['Reggie']
```

### First-class Citizen

* A first-class citizen in a given programming language is an **entity which supports all the operations generally available to other entitites.** These operations typically include:
  * being pased as an argument,
  * returned from a function,
  * modified,
  * and assigned to a variable
* An object with no restrictions on its creation, destruction, or usage
  * This includes the ability to be passed as an argument, returned from a function, and assigned to a variable