# Recursion

* `Recursion` is an alternative to traditional looping that allows you to do the same thing, just in a slightly different  way.
  * Not necessarily *better*, just *different*

* A traditional `while` loop:

```js
let number = 0;
while (number <= 12) {
  console.log(number);
  number += 2;
}
```

* Can be written out the same way with `recursion`

```js
function countEvenToTwelve(number) {
   if (number <= 12) {
     console.log(number);
     countEvenToTwelve(number+2);
   }
 }

countEvenToTwelve(0);
```

* Here's how to understand what's going on:

```js
function countEvenToTwelve(number) {
   if (number <= 12) {
     // Anything inside this conditional is called
     // The recursive case
     console.log(number);
     countEvenToTwelve(number+2);
   }
   // Anyting outside of the conditional is called
   // The base case
   // If true, we won't call the function again
 }

// Start here! Call the function
// With the starting number
countEvenToTwelve(0); 
```

* Whenever a recursive function calls itself, the input gets closer and closer to the **base case**.
* A function must have at least one recursive case and at least one base case in order to be recursive

 