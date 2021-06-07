# Intro to React

## Review of JS

* We need to make sure that we're up to date with everything JS to get the best handle on React

### Semicolons

* Use them. Always. Team semicolon.

### `const` and `let`

* We no longer user `var` in JS, and much prefer to use `const` or `let`

* Both are block-scoped, meaning that they only pertain to the block they're declared in - such as a function or `if` statement.

* `const` cannot be reassigned, however.
  * Don't forget `reassign` doesn't mean change in value for `arrays` and `objects`
  * We can `push` or assign values to these data types even when they're `const`

* Use `const` 99% of the time, unless the system tells you that you can't

### Closures

* A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**).  In otehr words, a closure gives you access to an outer function's scope from an inner function. In JS, closures are created every time a function is created, at function created time.

* For example:

```js
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

const add5 = makeAdder(5);
const add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12
```

* We define a function called `makeAdder` that takes a single argument `x` and returns a new function. The fucntion it returns takes a single argument `y` and returns the sum of `x` and `y`

* The variables `add5` and `add10` are **closures**. They share the same function body definition, but store different lexical environments (where `x` has different values)

* This has obvious parallels to Object Oriented Programming, where objects allow you associate data (the objet's properties) with one or more methods.

* This is useful on the web where much of front-end code is event-based. So we can define some behavior, attach it to an event that is triggered by user (click, keypress). The code is attached as a callback (a single function that is executed in response to the event).

* Some languages allow you to declare methods as *private*, meaning that they can be called only by other methods in the same class. JS does not provide a native way of doing this, but **it is possible to emulate private methods using closures**.

```js
const counter = (function() {
  let privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }

  return {
    increment: function() {
      changeBy(1);
    },

    decrement: function() {
      changeBy(-1);
    },

    value: function() {
      return privateCounter;
    }
  };
})();
```

### Expressions vs Statements

* Expressions resolve to a single value, such as calculating the value for `resut`:

```js
const result = someCondition ? 'Michael' : 'Bruce'
```

* Comparatively, an expression for this same line would look something like this:

```js
let result = null
if (someCondition) {
  result = 'Michael'
} else {
  result = 'Bruce'
}
```

### Functions

* We will use arrow functions for most things, but it's important to remeber that arrow functions **don't create context**, so `this` inside the arrow function is the same as the `this` on the outside

### Object Destructuring

* Object destructuring is a way to take an object and to pull out its internal properties into variables outside of the object.

```js
const obj = { x: 1, y: 2 }

// instead of:
const x = obj.x
const y = obj.y

// We can "destructure" the values into ordinary
// variables:
const { x, y } = obj
x // 1
y // 2

// you can use this all over the place, like function
// parameters. Notice how we're passing just one thing
// (an object) into the add function. If the function
// is expecting an argument, it can destructure the
// values right in the parameter list.
function add({ x, y }) {
  return x + y
}
add({ x: 3, y: 4 }) // 7
```

### Array Destructuring

* Works the same as object destructuring, but the way we destructure is tried to what value we get -- in other words, `first` is the variable in the destructure so it gets the first value of the array.

```js
const arr = ['michael', 'jackson']
const [first, last] = arr
first // michael
last // jackson
```

### Spread syntax

* We can use `...` to create properties from the properties of an existing object or array.

* So we can nest an array or object using the variable name with `...`

```js
// Let's say you have this array
const person = ['Michael', 'Jackson']

// If you were to add the above array to a new one like this:
const profile = [person, 'developer']

// The end result would be an array in an array like this:
profile // [['Michael', 'Jackson'], 'developer']

profile[0] // this is an array
profile[1] // this is the string 'developer'

// However, if we had made profile like this with ...
const profile = [...person, 'developer']

// Then the end result would be this:
profile // ['Michael', 'Jackson', 'developer']

// The same concept works with objects
const person = { first: 'Michael', last: 'Jackson' }
const profile = { ...person, occupation: 'developer' }
profile // { first: 'Michael', last: 'Jackson', occupation: 'developer' }
```

### Rest syntax

* Similar to spread syntax, rest is not used to build object or arrays, but to **break them down into pieces**

```js
const profile = { first: 'Michael', last: 'Jackson', occupation: 'developer' }
const { occupation, ...rest } = profile
occupation // developer
rest // { first: 'Michael', last: 'Jackson' }
```

### ES Modules

* A different way to import and export code from different files

```js
import SomeModule from 'some-module'
SomeModule.someMethod()

// more code here...

export default SomethingToExport
```

* Or using destructuring-like syntax on the import:

```js
import { someMethod } from 'some-module'
someMethod()

// more code here...

export default SomethingToExport
```

### Short-circuiting (&&)

* Most languanges will "short-circuit" when attempting to check the true or falseness of two expressions.

* This means we can chain our conditionals and some will fire up until a point.

```js
function one() {
  console.log('one was called')
  return false
}
function two() {
  console.log('two was called')
  return false
}

if (one() && two()) {
  console.log('Here we go!')
}

// The only output of this code is "one was called" because of
// short circuiting
```

## React

### Imperative vs Declarative Programming

* Imperative is the **how** of doing something, where as declarative is the **what** of doing something
  * Think of separating them in the following manner
    * How will give you exact details on how something is done
    * What will be more concered with what you want

* Many (if not all) declarative approaches have some sort of underlying imperative abstraction.

* In an interview, you might be asked questions that can have imperative and declarative answers. Creating a function that takes an array and adds all the values together.
  * Imperative - we create the `for` loop and add each value to the other
  * Declarative - we use a pre-built function, like `reduce` to do the work for us

### Components

* Components are the most important concept in teh React ecosystem. They are the building blocks of complex user interfaces. Each component can receive data and maintain state. When a component receives new data or changes it's state then it will return elements that will be rendered to the DOM

* Good Components are:

  * **Composable**: we can combine them together to build complex user interfaces
  * **Encapsulated**: we can build them in isolation
  * **Reuseable**: we can use them in different parts of our application * without duplicating code
  * **Testable**: we can isolate them for certain types of testing

* We should try to break down our apps into components that contain single-responsibility
* Then we should map our components into parent-children relationships.

* We can then declare the structure of the application using a language called JSX, which looks a lot like HTML or XML

### Props & State

* **Props** are the data passed from a parent component to a child component. Similar to passing arguments to a fucntion except a component cannot change the proops passed to it. **State** is private data managed internally by the component. A `render()` is triggered when a component changes its state.

* Good rules to check if it's props or state
  * Is it passed in from a parent via props? If so, it probably isn't state.
  * Does it remain unchanged over time? If so, it probably isn't state.
  * Can you compute it based on any other state or props in your component? If so, it isn't state.

## JS tooling

### Babel

* We use Babel to compile our new, ES2015+ JS into backwards compatible version of JS. This alows us to use brand new or not yet released features of the JS language in our apps today.

### Webpack

* Both CommonJS and ES^ Modules allow us to define our dependencies. CommonJS is not build into the browser and ES6 Modules are not fully supported by older browsers. Webpack pre-bundles all of our modules into one or more JS files.

* If we have several files with different functions, we can use `npx webpack` to create a bundle for our dependencies

* This will create a `bundle.js` file that has all of our dependencies and our other js files.

