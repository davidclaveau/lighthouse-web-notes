# Promises

* Before promises, we often saw **callback hell** to continuously move on to the next step in the even loop.

```js
functionCall('string', (err, message) => {
  anotherCall((err, message) => {
    // Here I am nested inside two callback calls.
    // This isn't good
    // And it ain't getting any better.
  });
});
``` 

* As we see in the survey-callbacks-example.js file, this cascade of function calls is both incredibly WET and can cause all kinds of issues with errors.

* What we can do is use **Promises** to create a cleaner version

```js
rlp.questionAsync('What do you think of Node.js? ')
  .then((answer) => {
    answers.push(answer);
    return rlp.questionAsync('What\'s your name? ');
  })
```

* Note that the first line doesn't have a semicolon:

```js
rlp.questionAsync('What do you think of Node.js? ') // <--
```

* That's because we're calling the `.then` *function* on the object that's called by the `promise`.

```js
  .then((answer) => {
```

* Promises can be either returned as *resolved* or *in error*

* This is the callback that's called if it's *resolved* in the 'positive' sense:

```js
//.then(
  (answer) => {
    answers.push(answer);
    return rlp.questionAsync('What\'s your name? ');
  }
// )
```

* And the promise goes down the list sequentially, and adding additional promises is quite easy to implement

* Finally, if there is an error we can throw a `catch` to report it

```js
.catch((err)=>{
  console.log(err);
});
```
---

## How Promises are Made

* Usually you are using promises from other packages

* Looking at the `promise-generator` file:

```js
const returnPromise = (value, delay = 1000) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(`yay! resolved!: ${value}`), delay);
  });
};
```

* Looking at the promises, we're passing a `value` and a `delay` (with a default parameter =1000)

* We're making a `new` Promise *object*
  * The new Promise  object is created by passing another callback
  * We have `resolve` and `reject`
  * We call `resolve` callback when the promise resolves, and `reject` if it returns rejected

* If we look back at the promise `intro.js` file, we can see how promises are called

```js
const promiseGenerator = require('./promise-generator');

const promise = returnPromiseGenerator.returnPromises('first promise', 4444);

console.log(promise);
```

* When we console log the promise, we see that the main thread ends and then, after 4444ms, the promise resolved
  * The promise doesn't show its message/value

* If we use a `setTimeout` that's greater than 4444ms and console.log the promise, we would see the promises' resolved value.

```js
setTimeout(()=>{
  console.log(promise);
},5000);

```

* But there's a better way:

```js
promise
  .then((data) => {
    console.log(data);
    return 'another thing';
  })
```

* `data` in the promise above is the `resolved` value of the promise, whereas the `return` doesn't return anything for us to look at.
  * A `return` statement in a promise allows you to chain your `.then` functions, so the next promise's `data` returns the `return` from the previous `.then`

```js
promise
  .then((data) => {
    console.log("data", data);
    return 'another thing';
  })
  .then((data2) => {
    console.log("data2:",data2);
  });

// (Output)
// data: data was resolved! yay!
// data2: another thing
```
---

## What to do with Errors

* We can use `.catch` to catch and print any errors from our promises

```js
.catch((err) => {
  // throw err;
  console.log("error:", err);
})

// error: doh, rejected (err)
```

* If there's an error in a `then`, you will skip the rest of the `then`s and go to `catch`

* If you're able to put in decent error messages, and have decent  error handling, it makes your code useful.
---

## Paralellizing Async Things

* `Promise.all` uses the Promise **class**

```js
  
const functions = require('./promise-generator');

const returnPromise = functions.returnPromise;
const returnRejectedPromise = functions.returnRejectedPromise;

const promiseOne = returnPromise('one', 1500);
const promiseTwo = returnPromise('two', 4000);
const promiseThree = returnPromise('three', 2000);
const promiseFour = returnPromise('four', 3000);

const promises = [promiseOne, promiseThree, promiseTwo, promiseFour];


Promise.all(promises) // Takes an array as a parameter
  .then(() => {
    console.log("success:", data)
  })
  .catch(() => {
    console.log("something was rejected", err)
  })
```

* We get back an array of `return` values

* The returned array correspond to the index value of the promises that are handed in

* If any one of them returns with an error (rejected), the system will call the `catch`.

* Then it will run when all promises resolve

* `Promise.race` will run differently, and will return the first promise in the event loop

```js
const functions = require('./promise-generator');

const returnPromise = functions.returnPromise;
const returnRejectedPromise = functions.returnRejectedPromise;

const randomDelay = () => Math.floor(Math.random() * 5000);

const promiseOne = returnPromise('one', randomDelay());
const promiseTwo = returnPromise('two', randomDelay());
const promiseThree = returnPromise('three', randomDelay());

const promises = [promiseOne,  promiseTwo, promiseThree];

Promise.race(promises)
  .then((data) => {
    console.log(data);
  });
```

* This will return either one, two, or three based on which value's delay was the shortest

---

## Creating your own Promises

```js
const returnRandomPromise = (value, delay = 1000) => {
  const num = Math.floor(Math.random() * 2);
  if (num < 1) {
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve(`yay! resolved!: ${value}`), delay);
    });
  }
  return new Promise((resolve, reject) => {
    setTimeout(() => reject(`doh! rejected: ${value}`), delay);
  });
};
```

* This is where we would want to refactor the code to create the `new` Promise with the `resolve` and `reject` and then create the conditionals to return them as needed.