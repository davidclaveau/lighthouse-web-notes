# Data Types

## Primitive Data in JS

* `null`, `undefined`, `string`, `number`, `boolean`, `symbols`, `bigInt` 
  *  symbols are guaranteed unique strings (not popular)
  * `bigInt` ends with `n`
* Inside `Number.MAX_SAFE_INTEGER` is a large number, but `bigInt` holds a larger number than this (4 quadrillion)
* Primitive data consist of single values
* They are immutable === unchangeable
  * unchangeable !== reassignable
  * `=` means we're replacing the value
* JS is "dynamically typed", which means the type of a variable is checked **during run-time**

```js
// Using a string method doesn't mutate the string
var bar = "baz";
console.log(bar);               // baz
bar.toUpperCase();
console.log(bar);               // baz

// Using an array method mutates the array
var foo = [];
console.log(foo);               // []
foo.push("plugh");
console.log(foo);               // ["plugh"]

// Assignment gives the primitive a new (not a mutated) value
bar = bar.toUpperCase();       // BAZ
```

### Data Structures in JS

* Collect related pieces of information together
* Arrays are these types of data types, so we can have several bits of information and access it through the **index position**
* In JS, there's **only two data structures** to remember: arrays and **objects**
* `const student = {};` is an **object literal**
* Objects are a collection of key-value pairs
Best practice is to declare objects with `const` in most cases
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
* Dot syntax will **not** work with dynamic values
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

## Passing Values

* Primitives don't pass themselves, they pass a *copy*
* When passing a primitive data type to a function, it gets a *copy* - but the primitive **outside of the function remains unaffected**
* Objects and arrays are **mutable**, meaning they can be changed

* JS will place a function into memory all at once
  * We keep objects and arrays into **reference** memory
  * *Pass by reference*
* A primitive is **passed by value**, so a copy is passed

* So when we pass arrays and/or objects into functions, it accesses that reference memory (**not a copy**), so the array and/or object is changed forever and for always.
