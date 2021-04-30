# Object Methods and This

* In JS and other languages, it's quite common to have properties that reference data
  * like strings, numbers, arrays, etc.


* What we can do is take an object, like this 

```js
const person = {
  firstName: 'Bob',  // <= property containing data
  lastName:  'Smith' // <= ditto
}
```
* And use the data to call multiple key-value pairs, which would come out like this:

```js
console.log(person.firstName);
// or ...
console.log('Hello, ' + person.firstName + ' ' + person.lastName);
```
* But this can be hard to read and clunky
* We can assign functions as values to obects. This allows us to create a cleaner code.

```js
const person = {
  firstName: 'Bob',
  lastName:  'Smith',
  fullName: function() {
    return this.firstName + ' ' + this.lastName;
  }
}

// Nice, now I can just say:
console.log('Hello, ' + person.fullName());
// And it's much "cleaner" every time I need their full name!
```

* Functions like this that are attached to objects are often referred to as a **method**. Other code can now invoke or call methods like these from outside the object, without having to know or care about how they run.

## `this` and its uses

* For now, we can use this as only needing to be used inside methods like we used above.
  * `this` refers to the object the method (function) was called on.
* There's more to `this` than what's described, but that's for another day!