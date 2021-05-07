# Object Oriented Programming (OOP)

* Object Oriented (OO) is a software paradigm that originated in the 1960s and grew in popularity with C++ in the 90s.

* Similar to what we've been doing to modularize our code and keep things DRY, OO does this.

* JS is not stricty OO in the way languages like Ruby or Python are

* Another paradigm is functional programming, which JS also encourages

---

## Review of OOP in JS

* In OOP, we use objects to group related variables and functions together to keep our code more oganized. Pieces that all relate to one element we place together in an object.

```js
const dog = {
  sound: "woof",
  dogBreed: "shih tzu",
  speak: function() {
    console.log(`${this.dogBreed} says ${this.dogSound}`);
  }
};
```

* An object is a collection of key-value pairs known as properties.

```js
const dog = {
  sound: "woof", // Property
  breed: "shih tzu" // Property
};
```

* An object has stuff it **can do** known as behavior, and this behavior takes the form of `methods` which we give to functions when they're tied to an object. Some `methods` might modify the object, some might ask the object for info, etc. 

## `this`

* We use this within a line of code as if it were a variable, but it's actually a keyword like `for` or `function`.

* `this` in JS is determined by the context at the time of the call and depends on how and where it was called.

* All we really need to know for OO in JS, is that when you ise `this` inside a method, `this` refers to the object that the method was called on:

```js
const dog = {
  sound: "woof",
  speak: function() {
    console.log(this.sound);
  }
};

dog.speak();
```