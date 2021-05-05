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

* A thing to note about Node modules and using `modules.export`

* If you're exporting multiple items with `modules.export`, then you can export it as an object.

```js
// fileA.js
modules.export = {
  myNumber: myNumber, // 42
  myString: myString, // "string"
  myFunction: myFunction // function(){}
}
```

* And in the other file, you can accept the object and use dot notation to access the key-value pairs

```js
// fileB.js
const myObjects = require('./fileA')

myObjects.myNumber // 42
myObjects.myString // "Answer"
```

* But we can shorthand this even more with ES6
  * Basically, we remove the value to the key, as they're the same

```js
// fileA.js
modules.export = {
  myNumber,
  myString,
  myFunction:
}
```

* And then we make the import in fileB to be an object
  * And we can remove the `myObjects` to match the exported keys


```js
// fileB.js
const { myNumber, myString, myFunction }= require('./fileA')

myNumber // 42
myString // "Answer"
```