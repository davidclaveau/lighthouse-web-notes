# Day 3 Notes

## Lecture Notes

### Primitive data in JS
* null, undefined, string, number, boolean, symbols, `bigInt` 
* symbols are guaranteed unique strings (not popular)
* `bigInt` ends with `n`
* inside `Number.MAX_SAFE_INTEGER` is a large number, but bigInt hold a larger number than this (4 quadrillion)
* Primitive data consist of single values
* They are immutable === unchangeable
  * unchangeable !== reassigned
  * `=` means we're replacing the value
* JS is "dynamically typed"

### Data structures in JS

* Collect related pieces of information together
* Arrays are these types of data types, so we can have several bits of information and access it through the **index position**
* In JS, there's only two data structures to remember: arrays and **objects**
* `const student = {};` is an **object literal**
* Objects are a collection of key-value pairs
* Best practice 8s to declare objects with `const` in most cases
  * Use `const` everywhere until JS tells you otherwise

```javascript
const student = {
  name: 'Alice',
  cohort: 'APR26',
  avgGrade: 90
};
```
* We can use either **square bracket syntax** or we can use **dot syntax** to find these values

```javascript
student["name"]; // square bracket syntax
student.name; // dot syntax
```
* Looking for a key in an object that doesn't exist will return `undefined`
* **Square syntax will work every time**
* Dot syntax will not work with dynamic values
* We can also contain other data structures in an object, such as arrays or other objects

```javascript
const student = {
  names: ['Alice', 'Tom'],
  cohort: 'APR26',
  avgGrade: 90,
  friend: {
    name: 'Dean',
    scores: [10, 15, 7]
  }
};

student.friend.scores[2] // Get the value 7
student['student']['friend']['scores'][2] // Get the value 7
```

* We can also have a function in an object

```javascript
const student = {
  names: ['Alice', 'Tom'],
  cohort: 'APR26',
  avgGrade: 90,
  friend: {
    name: 'Dean',
    scores: [10, 15, 7]
  },
  myFunc = function() {}
};

student.friend.scores[2] // Get the value 7
student['student']['friend']['scores'][2] // Get the value 7
```

* We can use dot or square bracket notation to change the object's values for a key

```javascript
student.cohort = 'MAY24' // change the cohort value to MAY24
```

---


### Passing Values

* Primitives don't pass themselves, they pass a copy
* When passing a primitive data type to a function, it gets a copy - but the primitive **outside of the function remains**
* Objects and arrays are **mutable**, meaning they can be changed

* JS will place a function into memory all at once
  * We keep objects and arrays into **reference** memory
* A prmitive is **passed by value**, so a copy is passed

* So when we pass arrays and/or objects into functions, it accesses that reference memory (**not a copy**), so the array and/or object is changed forever and for always.

---

### Functions in Objects

Example object:

```javascript
const snack = {
  flavour: 'zesty',
  spiciness: 7,
  texture: 'crunchy',
  amount: 20,
  dippable: true
};
```

* We have the abilitiy to add functions, and functions **define behaviour**

```javascript
const snack = {
  flavour: 'zesty',
  spiciness: 7,
  texture: 'crunchy',
  amount: 20,
  dippable: true,

  eatSome: function(amountEaten) {
    snack.amount = snack.amount - amountEaten;
    console.log(`there is ${snack.amount} left`)
  }

};

console.log(snack);
snack.eatSome(6); // When it calls the function, tells us there's 14 left
```

* While this works, we would actually want to avoid

```javascript
    snack.amount = snack.amount - amountEaten;
```

* This is because, at any point in our code, the `snack` variable may change and is outside of our control

* In JS, if you call a function inside an object, it will point to the object itself (to the left of the dot)
  * We can take advantage of `this` to update our code to ensure it's called correctly in all cases

```javascript
const snack = {
  flavour: 'zesty',
  spiciness: 7,
  texture: 'crunchy',
  amount: 20,
  dippable: true,

  eatSome: function(amountEaten) {
    this.amount = this.amount - amountEaten;
    console.log(`there is ${this.amount} left`)
  }

};

console.log(snack);
snack.eatSome(6); // When it calls the function, tells us there's 14 left
```
Another example with other functions calling functions in the same object:

```javascript
const snack = {
  flavour: 'zesty',
  spiciness: 7,
  texture: 'crunchy',
  amount: 20,
  dippable: true,

  eatSome: function(amountEaten) {
    this.amount = this.amount - amountEaten;
    console.log(`there is ${this.amount} left`)
  },
  goOnADiet: function(amount) {
    const reducedAmount = amount / 2;
    this.eatSome(reducedAmount); // Call the eatSome function within the object
  },
  changeTexture: function(newTexture) {
    this.texture = newTexture; // Change the texture to the newTexture value
  }
};
```

### Notes on Looping

* We can use `for...of` to loop through the values in an array
* We can use `for...in` to loop through the indexes (indices) of an array
* We can also use `for...in` to loop through the keys of an object
  * If you're trying to loop through the data values of each key, you would have to use **square bracket** as dot notation will look for a string and not work (`undefined`).