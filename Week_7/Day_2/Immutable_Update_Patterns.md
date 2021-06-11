# Immutable Update Patterns

## A **framework** vs a **library**
  
* Both are a set of functions
* A library is a set of functions that *you* call - you write the first file that calls the library
  * Think jQuery - you set up the code to read the file properly
* A framework relies on code that you don't write first - you run an application that's been written for you.
  * (That framework code looks for code you've written - writing code to be read by code)
  * React is a framework - it's going to run and look for your files regardless if they're there or not.

---

## Review 

* State cannot be set conditionally - it needs to be in the same order every time the page renders

```js
if (true) {
  const [] = useState(); // <-- Can't do this
}
```

* `useState` is a getter and a setter. The second argument always sets the datatype for the first argument.

* We use state locally for `input` to dynamically update the form with input that the user is typing

```js
// Have useState to continuously update the input
const [day, setDay] = useState('');

...

// Update the current value and any changes constantly
<input value={ day } onChange={ event => setDay( event.target.value )} />
```

---

## Valid Change Detection

```js
const user = {
  name: 'Alice',
  age: 40,
};

// Create a copy of user
const copy = user;

// We change the name of copy
copy.name = 'Bob';

// Print both
console.log(user);
console.log(copy);

// But our output will be 'Bob' object twice
// This is because it points to a reference -> {}
```

* We need a faithful original that we can compare to the changes made

* That's why we need to use `const` when we call `useState`.
  * `const` tells React that this thing cannot be modified, and it's the original value to compare to.
  * This is actually done with *clobbering*
    * Clobbering is when you have an object with keys of the same name

```js
const user = {
  name: 'Alice',
  age: 40,
  name: 'Bob'
}

// Bob 'clobbers' the name for Alice
// But 'Alice' is still there, just not used
```

* To do a pure copy - without overwriting the original `user`, we can use the `spread` operator (`...`)

```js
const user = {
  name: 'Alice',
  age: 40,
};

//Spread operator
const copy = {
  ...user,
  name: 'Bob',
}

// Print both
console.log(user);
console.log(copy);

// Output is the proper 'Alice' then 'Bob' objects
// Because 'Bob' clobbers 'Alice' in the copy
```

---

## Updating State and Infinite Loops

* If your component has an event handler that updates states, and you have an onClick that triggers an update to the state, a constant rerendering of state will occur.

* If you're seeing an infinite loop, it could be calling a function expression and not a callback

```js
<Header onClick={props.setDay(childDay)} />
```

* This will give onClick the expression to be used each onClick

* Whereas passing it a callback is preferable and won't cause infinite loops

```js
<Header onClick={() => props.setDay(childDay)} />
```