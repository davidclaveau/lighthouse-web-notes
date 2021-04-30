### Incremental development

### Pseudocode

* Pseudocode helps us with understanding *what* we want out of our code before we get into *how* we're going to do it
* We write it down in basic English to understand the process
* For example, with the LoopyLighthouse question, we can write the process like this:

```js
/*
Loop from 100 to 200:
  Let num = the current step in the loop
  If num % 3 is equal to 0 and num % 4 is equal to 0:
    Print "LoopyLighthouse"
  Else if num % 3 is equal to 0:
    Print "Loopy"
  Else if num % 4 is equal to 0:
    Print "Lighthouse"
  Otherwise
    Print num
  End if
End loop
*/
```

* Start with creating pseudo code for the start
* Take the instructions and separate the important steps

``` js
  // Extract an unlimited number of command line arguments
  // Go through each number
  // Calculate the sum => print it out
```
* Grab the edge cases and lay them out

```js
// We need at least two arguments
// Argument isn't a whole number, skip it
// If any argument is not a number, output an error message
```
* We first want to grab the arguments from the command line

```js
const args = process.argv
```

* Then `console.log` as you go along, and label it to ensure you know what you're logging to the console

```js
console.log("args:", args)
```

* We don't want the first two arguments, however, as that's the `node` location and the location of the file we're using with node
* Therefore we want to use `.slice(2)` to remove those first two values

```js
const args = process.argv.slice(2)
```

* This allows us to use the data we're passing in and process the values with a loop

```js
for (let i = 0; i < args.length; i++) {
  const num = args[i];
  console.log("num", num);
}

// Outputs each number passed from our command line
```

* Put we'd want to convert the `args` passed in to numbers, as they come through as a `string` initially.

* The difference between `let` and `const` is that `let` can **be reassigned** but `const` **cannot**.
  * **Reassigned** is different than "being changed". `Const` can be changed, but cannot be reassigned.
  * We can push data to a `const` and change the value as needed, but we can't reassign the variable, for example:

```js
const person = {
  name: 'David'
}

person.name = 'David Claveau' // This is fine!

person = {} // This is not, this reassigns the variable!

```

```js
let total = 0;

for (let i = 0; i < args.length; i++) {
  // convert the arg to number
  const num = args[i];

  // calculate the sum => print it out
  total = total + num;

  console.log("num", num, "total:", total);
}
```

* But our output is wrong

```js
num: 1 total: 01
num: 2 total: 012
num: 3 total: 0123
num: 4 total: 01234
num: 5 total: 012345
```

* We must then change the num to a `number`

```js
total = total + Number(num); // Number() converts the string into a number
```

* Now we can check for our edge cases

```js
// We need at least 2 arguments
if (args.length < 2) {
  console.log("Please provide at least 2 arguments");
} else {
  ... // execute the other code 
}
```

* Or we can use a `return` to the if statement to stop the code

```js
// We need at least 2 arguments
if (args.length < 2) {
  console.log("Please provide at least 2 arguments");
  return; // Ends the node snippet
}
```

* To check if the arguments are whole numners

```js
if (num % 1 === 0) {
  // A whole number is a multiple of 1
  // Floats would have a remainder
  // 15 % 1 === 0
  // 15.5 % 1 === 0.5
  //complete the code below
}
```

* Or we can use the `.isInteger()` method to check if the statement is `true`

```js
if (Number.isInteger(num)) {
}
```

* Buuuut there's also the issue that `typeof` can sometimes return `number` when it's not. That's why there's the `isNan()` function.

```js
if (isNaN(num)) {
}
```

### Single responsibility principle

* A function should only complete **a single thing**
* Our edge cases makes our function less useable
* We can create helper functions to complete these edge cases
* This makes our code **readable and modular**
* Our functions can be used multiple times

```js
const sum = function (numbers) {
  let total = 0;

  for (let i = 0; i < numbers.length; i++) {
    // convert the arg to number
    const num = numbers[i];

    // calculate the sum => print it out
    total = total + num;
  }
  return total;
}
```

```js
const convertToNums = function(numbers) {
  const outputNums = [];
  for (let num of numbers) {
    // Convert the arguments to numbers
    outputNums.push(Number(num));
  }
  return outputNums;
}
```

```js
const allNums = function(numbers) {
  for (let num of numbers) {
    // If any argument is not a number, output an error message
    if (isNan(num)) {
      console.log('please provide only numbers!');
      process.exit(); // Stop the node operation
    }
  }
  return outputNums;
}
```

```js
const allInt = function(numbers) {
  const outputNums = [];
  for (let num of numbers) {
    // If the number is not a whole number, skip it
    outputNums.push(isNum(num));
  }
  return outputNums;
}
```

* Then we can call all of our help functions at once:

```js
console.log('Total:', sum(allInt(allNums(convertToNums()));
```

---

### Back to git

* We use `git checkout main` and we can see the `main` branch has no changes
* We use `git checkout` to see the branches available
* We use `git checkout feature/sum` and see the code that we've created
* We use `git merge feature/sum` to push the changes to main

---