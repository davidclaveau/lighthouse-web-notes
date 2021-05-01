# Arrow Functions and This (Revisited)

* Arrow Function syntax can vary based on how simple the function is
  * We can remove curly brackets for one-line  functions, remove `return` and remove parameter parenthesis for single-parameter functions.

* Another thing to note is that Arrow Functions don't get assigned a value for `this` in the way that traditional function expressions do.
  * However, this limitation can be used as an advantage in certain situations - situations where it is beneficial to inherit the value of `this` from the lexical scope.

  For example:

```js
function() {

  let Person = function(name) {
    this.firstName = name;

    .this.sayHello = function() {
      setTimeout(() => {
        // We can use `this` to refer outside the
        // Arrow function
        console.log('Hello', this.firstName);
      }, 100);
    }
  }

  let p = new Person('David');
  p.sayHello();

})();

// Hello David
```

* Because arrow functions allow the lexical scope of `this`, it allows us to use the `this` keyword to apply to the object. 