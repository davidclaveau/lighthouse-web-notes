# Lotide Notes

### `console.assert()`

* We can use the `console.assert()` method
* This method writes an error message to the console if the assertion is `false`; if the assertion is `true`, nothing happens.

* We can also create our own functions to process arguments in a similar manner
* Lets the user know if it passed the test or failed

E.g. 

```
// FUCNTION IMPLEMENTATION
const assertEqual = function (actual, expected){
  // If actual === expected
  if (actual === expected){
    console.log("✅✅✅ Assertion Passed: " + actual + " === " + expected);
  } else {
    console.log("🛑🛑🛑 Assertion Failed: " + actual + " != " + expected);
  };
};

// TEST CODE
assertEqual("Lighthouse Labs", "Bootcamp");
assertEqual(1, 1);
assertEqual(1, 2);
assertEqual("Yes", "Yes");

```

Prints out the following: 

```
🛑🛑🛑 Assertion Failed: Lighthouse Labs != Bootcamp
✅✅✅ Assertion Passed: 1 === 1
🛑🛑🛑 Assertion Failed: 1 != 2
✅✅✅ Assertion Passed: Yes === Yes
```
---


## ES6 Template Literals