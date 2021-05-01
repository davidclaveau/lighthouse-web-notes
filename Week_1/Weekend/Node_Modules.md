# Node Modules

* Every file in Node is a module, and has access to the `module` object
* By `console.log(module)` you can see the module object for the node file:

```js
Module {
  id: '.',
  exports: {},
  parent: null,
  filename: '/Users/superman/codes/moduleCheck.js',
  loaded: false,
  children: [],
  paths: [ ... ] 
}
```

* The aspect we're looking at is the `exports: {}` object, which allows us to export certain parts of the file.

* We can use `require` with relative paths (like `./moduleCheck`) to connect the file.
  * We can omit the `.js` extension in the path, as it's unnecessary

* Therefore, we can have one file with the function:

```js
// ./moduleCheck.js

const sayHelloTo = function(person) {
  console.log(`Hello, ${person}`);
}

module.exports = sayHelloTo; // This exports the function
```

* And another file that uses that function:

```js
// main.js

// Looks for the exported function
const sayHelloTo = require('./moduleCheck');

sayHelloTo('Bernie')
```