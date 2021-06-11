# Data Fetching and Other Side Effects

## Pure Functions

* A pure function is a function that has no side effects - meaning that the same input will return the same output. This means there are no outside information or variables within the function

* The function only depends on its input parameters to return a value

* Doesn't produce changes beyond its scope (side-effect)

* Pure function:

```js
function someComputing(parameter) {

  parameter = parameter + 42;

  return parameter;
}
```

* Function with a side-effect:

```js
function someComputing(parameter) {

  parameter = parameter + 42;

  additional input = 43; // sending data outside the function

  return parameter;
}
```

### Side-Effects

* A secondary effect (an interaction with scope outside of the function) when running a function.
  * Setting timers or intervals
  * Modifying DOM elements not controlled by React
  * Network request
  * Connection to a socket server
  * Adding and removing event listeners
  * Loggin to the console

* Generally undesirable because they can introduce a lot of bugs

* In React, they can disrupt the normal component rendering

* Frameworks are considered *opinionated*, and they have preferences for the way things should be done

## useEffect Hook

* To better handle side-effects, React encapsulates them in a `useEffect` Hook

* `useEffect` has three potential actions:
  * Adding a side effect
  * Reinvoking the side effect (or not)
  * Cleaning up the side effect

* For example, connection to an API
  * Browser opens a connection to an API that's pulling in date
  * Cleaning this up might include *closing* the conncetion as the function is wrapping up

---

* Here's some code showing how to use `useEffect`

```js

import { useEffect } from 'react';


const ChildOne = (props) => {

  const [childDay, setChildDay] = useState('');

  // Every letter we type in our `input` will call this
  // every time (because we're tracking state). This 
  // determines if the side effects need to be managed.
  useEffect(() => {
    console.log("useEffect was called")
  });

  return (
    <div>
      <h1></h1>
      <input
        value={ChileOne}
        onChange={event => setChildDay( event.target.value ) }
      />
      <button onClick={ () => props.setDay(childDay)}>Set the Day</button>
    </div>
  );
};
```

* We need to call the second parameter of `useEffect`

```js
  useEffect(() => {
    console.log("useEffect was called")
  }, []); // Add an array to useEffect
```

* When we type in our input, we no longer have the console logs coming through with each letter.

* This second parameter `[]` is called the **dependency array** to signify what this should be called.
  * You put in a state variable, the state *getter*
  * (An empty array tells `useEffect` that this call doesn't depend on anything)

```js
  useEffect(() => {
    console.log("useEffect was called")
  }, [childDay]); // Specify the state getter we're using
```

* This will return us to what we had before, where each input calls the console logs.

* **Any code that's outside the scope of the component will be used in the `useEffect` hook.**

* The callback we use in `useEffect` can `return` one of two things:
  * `undefined`
  * Or, a function definition

```js
 useEffect(() => {

  // Somewhere up here we open the websocket

  return (() => {

    // Down here, we close the websocket
  })

}, [childDay]); 
```

---

