# Asynchronous Javascript

* We're going to create a function that simulates the movements of our robot

```js
const start =  Date.now();

// look
console.log("Look");

// getUp
console.log("Get up");

// doASickFlip
console.log("Do a Sick Flip!");

// walk
console.log("Walk");

// openTheDoor
console.log("Open the Door");

// walkThroughTheDoor
console.log("Walk through the Door");

// (End of "main thread")
console.log("I am done being programmed.");
```

* When a JS code is executed, it runs through the **main thread** sequentially, one thing after the other

* There's a thing with JS functions where a thing can be done **asynchronously**
  * JS does this by getting on with the other thing while one event is being processed and scheduled to happen later
    * I.e. completing the main thread, and then completing other functions in the `event loop`

## Using `setTimeout()`

* For example, we can use `setTimeout` to perform an action in *the future*

```js
// (setTimeout takes a callback and a time)
setTimeout( () => { console.log("Do something"); }, 3000 );
```

* The `setTimeout` is a method that *schedules* something to be completed in the future (in ms)

* JS takes two actions:
  * The *main thread*, where things happen sequentially
  * After completing the main thread, it runs the *scheduled tasks* in the `event loop`

* We want to consolidate everything in a function:

```js
const start =  Date.now();

function doAction(name){
  console.log("Sstarting to ", name);

  // (setTimeout takes a callback and a time)
  setTimeout( () => { console.log("Do something"); }, 3000 );
};

// look
doAction("Look")

// getUp
doAction("Get up");

// doASickFlip
doAction("Do a Sick Flip!");

// walk
doAction("Walk");

// openTheDoor
doAction("Open the Door");

// walkThroughTheDoor
doAction("Walk through the Door");

// (End of "main thread")
doAction("I am done being programmed.");
```

## Adding the time parameter for each function call

* Then we can add a `time` parameter to the `doAction` function

```js
function doAction(name, time){
  console.log("Starting to ", name);

  // (setTimeout takes a callback and a time)
  setTimeout( () => { console.log("Do something"); }, 3000 );
};
```

* And we can add `time` to our function calls, so it's passed into the function and can be called using each call's time argument:

```js
const start =  Date.now();

// Pass in a `time` parameter as well
function doAction(name, time){
  console.log("Start: ", name);

  //  Now our `time` in setTimeout is a value we pass in
  setTimeout(() => {
    console.log("End: ", name);
  }, time);
};

// look
doAction("Look", 2000)

// getUp
doAction("Get up", 6000);

// doASickFlip
doAction("Do a Sick Flip!", 3000);

// walk
doAction("Walk", 5000);

// openTheDoor
doAction("Open the Door", 2000);

// walkThroughTheDoor
doAction("Walk through the Door", 2000);

// (End of "main thread")
doAction("I am done being programmed.", 3000);
```

* Now we have things happening as scheduled, they're still occuring all at once
  * setTimeout() is pretty quick to process the timeout requests so it just all flies out to the console immediately

## Setting another parameter to call the next function

* What we want to do is schedule another action to start after the other has completed

* We add another parameter to call the next `doAction`

```js
const start =  Date.now();

// Add the `next` parameter
function doAction(name, time, next){
  console.log("Start: ", name);

  //  Now our `time` in setTimeout is a value we pass in
  setTimeout(() => {
    console.log("End: ", name);
    next();
  }, time);
};
```

* And change the doAction calls to be functions definitions themselves

```js
// look
const look = () => {
  doAction("Look", 2000)
}

// getUp
const getUp = () => {
  doAction("Get up", 6000);
}

// doASickFlip
const doASickFlip = () => {
  doAction("Do a Sick Flip!", 3000);
}

// walk
const walk = () => {
  doAction("Walk", 5000);
}

// openTheDoor
const openTheDoor = () => {
  doAction("Open the Door", 2000);
}

// walkThroughTheDoor
const walkThroughTheDoor = () => {
  doAction("Walk through the Door", 2000);
}

// (End of "main thread")
const = () => {
  doAction("I am done being programmed.", 3000);
}
```

* But this doesn't do anything yet, we have to pass the next functions

```js
const start =  Date.now();

// Add `next` to call the other functions
function doAction(name, time, next){
  console.log("Start: ", name);

  //  Now our `time` in setTimeout is a value we pass in
  setTimeout(() => {
    console.log("End: ", name);
    if (null !== next) {
      next();
    }
  }, time);
};

// look
const look = () => {
  doAction("Look", 2000, getUp)
}

// getUp
const getUp = () => {
  doAction("Get up", 6000, doASickFlip);
}

// doASickFlip
const doASickFlip = () => {
  doAction("Do a Sick Flip!", 3000, walk);
}

// walk
const walk = () => {
  doAction("Walk", 5000, openTheDoor);
}

// openTheDoor
const openTheDoor = () => {
  doAction("Open the Door", 2000, walkThroughTheDoor);
}

// walkThroughTheDoor
const walkThroughTheDoor = () => {
  doAction("Walk through the Door", 2000);
}

look(); // Start event cascade

// (End of "main thread")
const = () => {
  doAction("I am done being programmed.", 3000);
}
```

* Now the scheduled tasks will occur right after the other

## Using `setInterval()`

* But this is still quite a rigid framework, so we would want to separate and have these functions called at specific times (at our own discretion)

* We can do this using `setInterval()` to do this

```js
//  Takes a callback and time in ms
setInterval( () = {}, 777 );
```

* This allows us to schedule functions to be interspersed with other functions

```js
const start =  Date.now();

// Pass in a `time` parameter as well
function doAction(name, time, next){
  console.log("Start: ", name);

  //  Now our `time` in setTimeout is a value we pass in
  setTimeout(() => {
    console.log("End: ", name);
    if (null !== next) {
      next();
    }
  }, time);
};

// look
const look = () => {
  doAction("Look", 2000, null)
}

// getUp
const getUp = () => {
  doAction("Get up", 6000, doASickFlip);
}

// doASickFlip
const doASickFlip = () => {
  doAction("Do a Sick Flip!", 3000, walk);
}

// walk
const walk = () => {
  doAction("Walk", 5000, openTheDoor);
}

// openTheDoor
const openTheDoor = () => {
  doAction("Open the Door", 2000, walkThroughTheDoor);
}

// walkThroughTheDoor
const walkThroughTheDoor = () => {
  doAction("Walk through the Door", 2000);
}

// We can add the `look()` function to be the callback
setInterval( () = {
  look();
  }, 777 ); // This keeps running every 777ms

getUp(); // Is now set to begin the cascade

// (End of "main thread")
const = () => {
  doAction("I am done being programmed.", 3000);
}
```

* This prints with having `look()` run continuously throughout the program
  * The other doAction functions are callled and appear as scheduled
  * But `look()` keeps going repeatedly, in that `setInterval()` interval time

* Don't forget that scheduled tasks cannot occur until *after* the main thread finishes

## Callback Looping using our `Look()`

* We can also use callback loops to continue the `look` function

```js
//  ...

// Tell look to loop on itself
const look = () => {
  doAction("Look", 500, look)
}

// ...

// We can comment this out:
/* 
setInterval( () = {
  look();
  }, 777 ); // This keeps running every 777ms
*/

// And instead call `look()` to repeatedly call itself in the scheduler thread

look(); //  Call `look` to look every 500ms, looping continuously

getUp(); // Is now set to begin the cascade

// (End of "main thread")
const = () => {
  doAction("I am done being programmed.", 3000);
}
```

* Callbacks are used to process data that are being used by the scheduler