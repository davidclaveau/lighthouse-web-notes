# Day 2 Notes

## Lecture - The Dev Workflow
  
### Curriculum Overview
* **Week 1** - FOCAL
  * Functions, Objects, Conditionals, Arrays, Loops
  * Friday will have Programming Test #1 (Mock) && Recursion

* **Week 2** - Asynchronous control flow (flowbacks, promises)
* **Week 3** - first web app -- HTTP Servers, Express.js, Cookies, Basic HTML & Forms
  * TinyApp - URL Shortener (Bit.ly clone)
* **Week 4** - Front end and client-side JS.
* **Week 5** - Relational Databases, SQL, Data Design, Postgres
  * Tweeter (Twitter Clone)
  * Week 5 will have a Midterm  Kickoff

* **Week 7 to 10** - Single page apps with React, Webpack, Babel, JSX
  * Interview Scheduler app
  * Real-time apps with WebSockets
  * Automated Testing for Web Applications
  * Class-based / Classical OOP with Ruby
  * Ruby on Rails
  * Adopting Existing Code
  * Research and write a blog post and summary

* **Week 11 to 12**  - Final Project
  * This time, you define the teams, stack, and requirements
  * No lectures, but some advanced topic lectures

**Three Tech Interviews**
* Weeks 2, 4, and 9
* One-on-one session with a mentor to show that you're keeping up

**Quizzes**
* Assesments of all your work

---

### Tools

* Useful Add-Ons
  * Eslint
  * Bracket Pair Colorizer extension in VS code
  * Prettier (not recommended for the first few weeks)

---

### Version Control

**Git** - What, why git?

* Repositories (one repo per project)
* Save milestones
* Keeps a history of your code (commits)
* Backup copy on github
* Work better as teams, branches
* Do use git
* You **will have to use** git in team projects

**Notes on git**

* Using `ls -al ` will show us the .git folder, letting us know what's initialized with git
* `git add` will add the files to the staging area
  * red means it's not added yet; green means it's been added
* `git commit -m "..."` needs to have a meaningful message
* `git log` will show the history of the commits, the author, the date.
* `git branch` to learn which branch we're on
* `git remote -v` shows which remote we're connected to
* `git remote add origin <URL>` will connect the folder/files to the repo on GitHub
* `git remote rm origin` will remove the folder/files from the repo on GitHub
* `git branch` will indicate which branch you're on
  * `git branch -M main` will allow you to change the name of the branch to "main"
* `git push -u origin main` will set the upstream to the main branch 
  * `-u` allows it to be connected to the main branch upstream, so you can use just `git push` in the future (without `origin main`).
* `git checkout` to change the branch
  * `git checkout -b (name)` allows you to create a new branch the first time

(if you're using HTTPS for the repo, you will be asked to authenticate each time with each push or pull; SSH is easier to use as it authenticates automatically)

---

### Incremental development

* Start with creating pseudo code for the start
* Take the instructions and separate the important steps

``` javascript
  // extract an unlimited number of command line arguments*
  // goes through each number*
  // calculate the sum => print it out
```
* grab the edge cases and lay them out

```javascript
// we need at least two arguments
// argument isn't a whole number, skip it
// if any argument is not a number, output an error message
```
* We first want to grab the arguments from the command line

```javascript
const args = process.argv
```

* Then console log as you go along, and label it to ensure you know what you're logging to the console log

```javascript
console.log("args:", args)
```

* We don't want the first two arguments, however, as that's the `node` location and the location of the file we're using in node.
* Therefore we want to use something like `.slice(2)` to remove those first two values

```javascript
const args = process.argv.slice(2)
```

* This allows us to use the data we're passing in and process the values with a loop

```javascript
for (let i = 0; i < args.length; i++) {
  const num = args[i];
  console.log("num", num);
}
```

* Put we'd want to convert the `args` passed in to numbers, as they come through as a `string` initially.

* The difference between `let` and `const` is that `let` can **be reassigned** but `const` **cannot**.
  * **Reassigned** is different than "being changed". `Const` can be changed, but cannot be reassigned.

```javascript
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

```javascript
num: 1 total: 01
num: 2 total: 012
num: 3 total: 0123
num: 4 total: 01234
num: 5 total: 012345
```

* We must then change the num to a `number`

```javascript
total = total + Number(num); // Number() converts the string into a number
```

* Now we can check for our edge cases

```javascript
// We need at least 2 arguments
if (args.length < 2) {
  console.log("Please provide at least 2 arguments");
} else {
  ... // execute the other code 
}
```

* Or we can use a `return` to the if statement to stop the code

```javascript
// We need at least 2 arguments
if (args.length < 2) {
  console.log("Please provide at least 2 arguments");
  return; // Ends the node snippet
}
```

* To check if the arguments are whole numners

```javascript
if (num % 1 === 0) {
  // A whole number is a multiple of 1
  // Floats would have a remainder
  // 15 % 1 === 0
  // 15.5 % 1 === 0.5
  //complete the code below
}
```

* Or we can use the `.isInteger()` method to check if the statement is `true`

```javascript
if (Number.isInteger(num)) {
}
```

* Buuuut there's also the issue that `typeof` can sometimes return `number` when it's not. That's why there's the `isNan()` function.

```javascript
if (isNaN(num)) {
}
```

### Single responsibility principle

* A function should only complete **a single thing**
* Our edge cases makes our function less useable
* We can create helper functions to complete these edge cases
* This makes our code **readable and modular**
* Our functions can be used multiple times

```javascript
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

```javascript
const convertToNums = function(numbers) {
  const outputNums = [];
  for (let num of numbers) {
    // Convert the arguments to numbers
    outputNums.push(Number(num));
  }
  return outputNums;
}
```

```javascript
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

```javascript
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

```javascript
console.log('Total:', sum(allInt(allNums(convertToNums()));
```

---

### Back to git

* We use `git checkout main` and we can see the `main` branch has no changes
* We use `git checkout` to see the branches available
* We use `git checkout feature/sum` and see the code that we've created
* We use `git merge feature/sum` to push the changes to main

---

## Core Work - Notes

### Pseudocode

* Pseudocode helps us with understanding *what* we want out of our code before we get into *how* we're going to do it
* We write it down in basic English to understand the process
* For example, with the LoopyLighthouse question, we can write the process like this:

```
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
```
* And then translate the pseudocode into proper javascript
```javascript
for (let num = 100; num <= 200; num++) {
  if (num % 3 === 0 && num % 4 === 0) {
    console.log("LoopyLighthouse");
  } else if (num % 3 === 0) {
    console.log("Loopy");
  } else if (num % 4 === 0) {
    console.log("Lighthouse");
  } else {
    console.log(num);
  }
}
```

### Refactoring
* But we can always refactor our code to try and make it DRY or more efficient. That's where we end up with a final code like this:

```javascript
for (const num of nums) {
  let output = "";

  if (num % 3 === 0) {
    output += "Loopy";
  }
  if (num % 4 === 0) {
    output += "Lighthouse";
  }
  console.log(output === "" ? num : output);
}
```

### Function Best Practices

#### Growing Functions

* When we write a function, it's sometimes tempting to be clever or do everything in one place
* But, using the **single responsibility princible**, we would want to break up a function to complete one thing at a time
  * This means having helper functions as needed to complete separate processes
  * *A useful principle is to not add cleverness unless you are absolutely sure you’re going to need it. It can be tempting to write general “frameworks” for every bit of functionality you come across. Resist that urge. You won’t get any real work done—you’ll just be writing code that you never use.*

#### Functions and Side Effects

* Functions can be roughly divided into those that are called for their **side effects** and those that are called for their **return value** (though you can have both in a function).
  * Functions that create values are easier to combine in new ways than unctions that directly perform side effects

* A **pure function** is a specific kind of value=producing function that not only has no sde effects, but also doesn't rely on side effects from other code.
  * Doesn't read gloabl bindings
  * When called with same arguments, always produces the same value (and doesn't do anything else).

#### Conventions for Naming Functions

* avoid generic names, like 'data' or 'run'
* name your functions beginning with **action** words, like `createUser` or `sendUserData`
* be consistent with your naming conventions
* if you're joining an existing project, observe and adapt any existing conventions.

#### Global-scoped

* A **globally-scoped** variable is available everywhere in the codes
* A **locally-scoped** variable would only be available in the function in which it's defined.

* It is a best practice to **Avoid reading out scope variables** in a function. If a function needs some information / data, then that data should instead be passed in as a **parameter**, making it available within the function's *inner scope*
* You should create a function that accepts global variables as a parameter / argument because the global variable may have changed from writing the function initially. **This also enables the function to be more dynamic, as it allows the argument passed in to be any number of variables in the code.**

## Getting Comfortable with Error Messages

* When looking at errors in the Terminal, we may see something like this:

```
    at exports.runInThisContext (vm.js:73:16)
    at Module._compile (module.js:443:25)
    at Object.Module._extensions..js (module.js:478:10)
    at Module.load (module.js:355:32)
    at Function.Module._load (module.js:310:12)
    at Function.Module.runMain (module.js:501:10)
    at startup (node.js:129:16)
    at node.js:814:3
```

* This is called a **stack trace** and shows the state of our program when the error occurred.
* It's possible that errors may not come from our file, and might be coming from something outside of our code, but usually it's a safe bet to assume the error is not caused by someone else's code but by ours alone - seeing as it's a work in progress.

#### Debugging Errors

* Debugging errors means that we break down our code into components
* We should look for places where the data undergoes changes and use `console.log` to print out what's happening
* From there, we can ensure the output is correct and make edits as needed

## Double Equals, Triple Equals, Type Coercion, and You

* There are two ways to compare values in JS. We can use either the `===` operator or the `==` operator.
  * The `===` operator compares the **type** of those values

```javascript
23 === "23" // returns false, as they are different types
23 == "23"  // returns true
```

* This is because with the `==` operator will attempt to force the two values to be of the same type, if possible. This is called **type coercion**.
* Similarly, we would want to use `!==` and not `!=` in most cases as the `!==` will check data type and avoid type coercion.

#### Truthy and Falsey

* Comparing two values in JS will either return `true` or `false`.
* Using the `==` operator can give you a weird answer

```javascript
0 == false // returns true
```

* Everything in JS has an inherent Boolean value, commonly referred to as `Truthy` or `Falsey`
* Most things are considered Truthy, except the following:

```javascript
// All of the following are inherently falsey:

False
// False is False. Makes sense, right?

0
// 0 is the only falsey Number

""
// an empty string is falsey

null
// a null, or empty value, is falsey.

undefined
// an object that has not been defined is considered falsey.

NaN
// Not a Number. You'll learn more about NaN as we go on.
```