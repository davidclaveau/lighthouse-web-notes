# Mocha and Chai

## BDD (Behavior Driven Development)

* BDD is a process that emerged from test-driven development in 2006.
  * Covers the entire life cycle of the app development process, from panning the project to writing the code
* BDD encourages you to specify the behavior of your app in terms of user stories, which are broken down into scenarios that can be built and tested.

### BDD Frameworks

* When we write tests, we would want to write data that's input into the code and then provide the assertion with the outcome that *should* come out of it.

## Mocha and Chai

* Mocha can be used or browser-based testing or Node.js testing.
* Mocha is the library that allows us to run tests, and Chai contains some helpful functions that we'll use to verify our test results.

* Testing on Node.js vs Testing in the browser
  * For Node, you don’t need the test runner file.
  * To include Chai, add `var chai = require('chai');` at the top of the test file.
  * Run the tests using the mocha command, instead of opening a browser.

* You should put your tests in a separate directory from your main code files, this makesit easier to structure them
  * Have a directory called `test/` in your project's root directory, then you have `test/someModuleTest.js`

* To test in a <u>browser</u>, we need a **test runner** page. The page loads Mocha, the testing libraries, and our actual test files.

* Using Node.js, you only need to run the command `mocha`

* Test runner for browser testing:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Mocha Tests</title>
    <link rel="stylesheet" href="node_modules/mocha/mocha.css">
  </head>
  <body>
    <div id="mocha"></div>
    <script src="node_modules/mocha/mocha.js"></script>
    <script src="node_modules/chai/chai.js"></script>
    <script>mocha.setup('bdd')</script>

    <!-- load code you want to test here -->

    <!-- load your test files here -->

    <script>
      mocha.run();
    </script>
  </body>
</html>
```

The basics of testing in browser:

  * We load Mocha’s CSS styles to give our test results nice formatting.
  * We create a div with the ID `mocha`. This is where the test results are inserted.
  * We load Mocha and Chai. They are located in subfolders of the `node_modules` folder since we installed them via npm.
  * By calling `mocha.setup`, we make Mocha’s testing helpers available.
  * Then, we load the code we want to test and the test files. We don’t have anything here just yet.
  * Last, we call `mocha.run` to run the tests. Make sure you call this after loading the source and test files.

* Then we can create a new test file.
  * For example, a file named `test/arrayTest.js` to test basic array functionality.

* First you have to have a `describe` block:

```js
describe('Array', function() {
  // Further code for tests goes here
});
```

* Describe is used to group individual tests. Thefirst parameter should indicate what we're testing, for exampe `'Array'`

* Then we need to have `it` blocks:

```js
describe('Array', function() {
  it('should start empty', function() {
    // Test implementation goes here
  });

  // We can have more its here
});
```