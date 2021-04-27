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

## Back to git

* We use `git checkout main` and we can see the `main` branch has no changes
* We use `git checkout` to see the branches available
* We use `git checkout feature/sum` and see the code that we've created
* We use `git merge feature/sum` to push the changes to main

