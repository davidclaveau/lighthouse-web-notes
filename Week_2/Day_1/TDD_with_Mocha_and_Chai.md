# TDD with Mocha and Chai

* Think about how we've been coding so far:

```js
const sayHello = (to) => {
  console.log("test:", `Hello ${to}`);
};

sayHello('World');

// test: Hello World
```

* Let's say we have errors in our code, like having missing syntax, we would check what prints in the terminal and fix it

* This is known as **error-driven development** and is the default of developing.
  * Error message, development, error message, development, and so on...

* We're looking to use **test-driven development**.

* Think about functions as input, processing, and output

* And consider a function as a *contract*
  * A line of code calls the function (`sayHello()`) and passes the argument as **input**
  * It **processes**
  * We expect something to happen using that function while passing that argument (**output**)

* Quick sidenote: we can use `nvm` to switch between different node versions

```js
// Bring in the assert library
const assert = require('assert').strict;

const sayHello = (to) => {
  return `Hello ${to}`;
};

const actual = sayHello("World");
const expected = "Hello World";

// To assert means that what follows is true
// These two things are the same
assert.equal(actual, expected)
```

* If we run `assert.equal(actual, expected)` and it is `true`, nothing will happen
* If it runs and it's `false`, we are thrown an error by the `assert.equal` function

* We would like to strip out our tests and keep them in a separate file away from the main  code
  * Keep the main code clean and more efficient

* So our main file looks like this:

```js
const assert = require('assert').strict;

const sayHello = (to) => {
  return `Hello ${to}`;
};

// This allows outside access to this function
module.exports = sayHello;
```

* And then in our test file, we would place the test code

```js
const assert = require('assert').strict;

// This looks to the file and imports the function
// using that module.export = sayHello;
const sayHello = require('./hello-wold')
// (./) means we're calling in the same directory

const actual = sayHello("World");
const expected = "Hello World";

// To assert means that what follows is true
// These two things are the same
assert.equal(actual, expected)
```

* Important thing to note, we can `module.exports` multiple functions in an object

```js
const sayHello = (to) => {
  return `Hello ${to}`;
};

const sayGoodbye = (to) => {
  return `Goodbye ${to}`;
};

// Create an object with the functions
const exportables = {
  sayHello : sayHello,
  sayGoodbye: sayGoodbye
}

// Now we're exporting an object instead of the individual function
module.exports = exportables;
```

* But now we can change the `require` for the other file.

```js
const assert = require('assert').strict;

// Change the variable name to be more appropriate
const mainFunctions = require('./hello-wold')

// Use dot notation to access the function in the object
const actual = mainFunctions.sayHello("World");
const expected = "Hello World";

assert.equal(actual, expected)
```

* We create a new directory for all of our tests, called `test/`
  * We move our `hello-world-test.js` file into this test file

* We then have to rename our `require()` to point to the right directory and file
  * If `test` is in the directory, we just have to go up one directory using `../`

```js
const mainFunctions = require('../hello-wold')
```

Now we can create some tests using the `mocha` framework
  * This uses the `describe` and `it` functions
  * `It` is an arbitrary test that writes a string when I expect it to

```js
//What it's testing
describe('Main functions test', () => {
  // What the test should output
  it ('writes Hello World when I expect it to', () => {
    const actual = mainFunctions.sayHello("World");
    const expected = "Hello World";

    // What actually happens
    assert.equal(actual, expected)
  });
});
```

* And this shows in terminal:

```
main functions test
  ✓ 1) writes Hello World when I expect it to.

1 passing (45ms)
```