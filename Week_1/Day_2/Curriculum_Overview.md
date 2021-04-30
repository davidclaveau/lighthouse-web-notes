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