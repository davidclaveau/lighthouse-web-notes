# Custom Hooks

* Building a custom hooks lets you extract component logic into reusable functions

* Moves repetitive or complex code out of our components

* Custom hooks usually use other React hooks (`useState`, `useEffect`, etc.)

* Think of them like helper functions for React (helper functions cannot use hooks).

* React Rule: React Hooks must start with the prefix `use`

* Multiple instances of the same custom hook **do not** share state

* Custom hooks can return anything you want (value, array, object, function)

---

* Let's make a counter in React that adds, subtracts, and resets

`App.js`
```js
import 'App.css';
import { useState } from 'react';


function App() {
  const [counter, setCounter] = useState(0);

  const increment = function() {
    setCounter(counter + 1)
  };
  const decrement = function() {
    setCounter(counter - 1)
  };
  const clear = function() {
    setCounter(0)
  };

  return (
    <div className="App">
      <h4>React Custom Hooks</h4>
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={clear}> 0 </button>
      <div>
        {counter}
      </div>
    </div>
  );
};

export default App;
```

* What if we wanted to reuse this code in another section? We would have to write the code again.

* *Or*, we could write a custom hook in a separate file

* All we have to do is copy and paste the function in the file.

`useCounter.js`
```js
const useCounter = function() {
    const [counter, setCounter] = useState(0);

    const increment = function() {
      setCounter(counter + 1)
    };
    const decrement = function() {
      setCounter(counter - 1)
    };
    const clear = function() {
      setCounter(0)
    };

    // We can return an object with the values we need
    return {counter, increment, decrement, clear};
};

export default useCounter;
```

* Back in App.js, we can import the custom hook.

`App.js`
```js
import 'App.css';
import { useState } from 'react';

// Have to import the custom hook
import useCounter from 'hooks/useCounter';


function App() {

  // We assgned the values from useCounter
  // And the function can be used as before
  // Returning an object allows you to use specific
  // values if needed
  const {counter, increment, decrement, clear} = useCounter();

  return (
    <div className="App">
      <h4>React Custom Hooks</h4>
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={clear}> 0 </button>
      <div>
        {counter}
      </div>
    </div>
  );
};

export default App;
```

* What if we have this same counter on the same page, but we want to *separate* them?

* We can change the object we're using for `useCounter` to return an array instead

* This assigns the function values to that index in the array

`useCounter.js`
```js
const useCounter = function() {
    const [counter, setCounter] = useState(0);

    const increment = function() {
      setCounter(counter + 1)
    };
    const decrement = function() {
      setCounter(counter - 1)
    };
    const clear = function() {
      setCounter(0)
    };

    // We can return an array instead
    return [counter, increment, decrement, clear];
};

export default useCounter;
```

* Then we can just rename the variables for the array in `App.js` to match the returned array's values

`App.js`
```js
import 'App.css';
import { useState } from 'react';
import useCounter from 'hooks/useCounter';


function App() {

  // Change the objects to arrays, and call them separately
  const [counter, increment, decrement, clear] = useCounter();
  const [counter2, increment2, decrement2, clear2] = useCounter();

  return (
    <div className="App">
      <h4>React Custom Hooks</h4>
      
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={clear}> 0 </button>
      <div>
        Counter 1 = {counter}
      </div>

      <button onClick={increment2}> + </button>
      <button onClick={decrement2}> - </button>
      <buttononClick={clear2}> 0 </button>
      <div>
        Counter 2 = {counter2}
      </div>
    </div>
  );
};

export default App;
```

---

## Data Fetching and Custom Hooks

* Let's fetch some quote data

* We can set the quote if we get it

* We can also set error and pending if those are occurring during the process

```js
import 'App.css';
import { useState, useEffect } from 'react';
import useCounter from 'hooks/useCounter';
import axios from 'axios';


function App() {
  const {counter, increment, decrement, clear} = useCounter();
  const [quote, setQuote] = useState("");
  const [error, setError] = useState("");
  const [pending, setPending] = useState(true);

  // Use axios to get the quotes
  useEffect(() => {

    // Reset the states every time
    setQuote("");
    setError("");
    axios.get("https://api.kanye.rest")
      .then(res => {
        setQuote(res.data.quote);
      })
      .catch(err => {
        // Set error if exists
        setError(err.message);
      })
      // Always run pending
      .finally(() => {
        setPending(false);
      });

      // Set it to be triggered on the counter change
  }, [counter]);

  return (
    <div className="App">
      <h4>React Custom Hooks</h4>
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={clear}> 0 </button>
      <div>
        Counter = {counter}
      </div>

      <ul>
        <li>
          <!-- Only show pending message if pending exists -->
          <!--  Only show error if error exists -->
          {pending && "Fetching Quote"}
          {error && error}
          {quote && quote}
        </li>
      </ul>

    </div>
  );
};

export default App;
```

* But we can reuse this axios function to use it for similar quote fetching

* We can recreate the function in a separate file

`useAxios.js`
```js
import { useState, useEffect } from 'react';
import axios from 'axios';

// Pass the url as a parameter for multi-function
const useAxios = function(url) {
  const {counter, increment, decrement, clear} = useCounter();
  const [body, setBody] = useState(null);
  const [error, setError] = useState(null);
  const [pending, setPending] = useState(true);
  
  useEffect(() => {
    setBody("");
    setError("");
    axios.get(url)
      .then(res => {

        // Make this more general and just grab data
        // in App.js, we'll need to specify that we
        // only want the quote
        setBody(res.data);
      })
      .catch(err => {
        setError(err.message);
      })
      .finally(() => {
        setPending(false);
      });

      // React will warn you that you should set a dependency
      // In case the URL changes on you in the future
  }, [url]);

  return {quote: body, error, pending}
};

export default useAxios;
```

* And import the function in `App.js`

`App.js`
```js

// We can remove the { useEffect, useState }
// We can also remove axios import
import 'App.css';
import useCounter from 'hooks/useCounter';
import useAxios from 'hooks/useAxios';

function App() {
  const {counter, increment, decrement, clear} = useCounter();
  const {body, error, pending} = useAxios("https://api.kanye.rest");

  return (
    <div className="App">
      <h4>React Custom Hooks</h4>
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={clear}> 0 </button>
      <div>
        Counter = {counter}
      </div>

      <ul>
        <li>
          {pending && "Fetching Quote"}
          {error && error}
          <!-- Specify what part of data we want -->
          {body && body.quote}
        </li>
      </ul>

    </div>
  );
};

export default App;
```

---

* Let's make a new component that uses our new hook

* A whole component dedicated to Kanye quotes

`KanyeQuote.js`
```js
import useAxios from 'hooks/useAxios'

const KanyeQuote = function() {
  const {body, error, pending} = useAxios("https://api.kanye.rest");

  return (
    <li>
      {pending && "Fetching Quote"}
      {error && error}
      {body && body.quote}
    </li>
  );
};

export default KanyeQuote;
```

* Then we can remove the section in our App.js that is now being taken care of by KanyeQuote component

`App.js`
```js
import 'App.css';
import useCounter from 'hooks/useCounter';
import KanyeQuote from 'KanyeQuote';

function App() {
  const {counter, increment, decrement, clear} = useCounter();

  return (
    <div className="App">
      <h4>React Custom Hooks</h4>
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={clear}> 0 </button>
      <div>
        Counter = {counter}
      </div>

      <ul>
        <KanyeQuote />
      </ul>

    </div>
  );
};

export default App;
```

---

* We can do the same with a different quoting API

`OfficeQuote.js`
```js
import useAxios from 'hooks/useAxios'

const OfficeQuote = function() {
  const {body, error, pending} = useAxios("https://www.officeapi.dev/api/quotes/random");

  return (
    <li>
      {pending && "Fetching Quote"}
      {error && error}
      <!-- Again, we have to change what we need from the API -->
      <!-- This API returns data.data.content -->
      {body && body.data.content}
    </li>
  );
};

export default OfficeQuote;
```

* And use the component in `App.js`

`App.js`
```js
import 'App.css';
import useCounter from 'hooks/useCounter';
import KanyeQuote from 'KanyeQuote';
import OfficeQuote from 'OfficeQuote';

function App() {
  const {counter, increment, decrement, clear} = useCounter();

  return (
    <div className="App">
      <h4>React Custom Hooks</h4>
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={clear}> 0 </button>
      <div>
        Counter = {counter}
      </div>

      <ul>
        <KanyeQuote />
        <OfficeQuote />
      </ul>

    </div>
  );
};

export default App;
```

---

* Say we want to bring back different information from our API call, such as the character who said the quote

`OfficeQuote.js`
```js
import useAxios from 'hooks/useAxios'

const OfficeQuote = function() {
  const {body, error, pending} = useAxios("https://www.officeapi.dev/api/quotes/random");

  return (
    <li>
      {pending && "Fetching Quote"}
      {error && error}
      <!-- Again, we have to change what we need from the API -->
      <!-- This API returns data.data.content -->
      {body &&
        <>
          <div>
            {body.data.content}
          </div>
          <span>{body.data.character.firstname}</span>
        </>
      }
    </li>
  );
};

export default OfficeQuote;
```

---

* Only show when our Counter is even

`App.js`
```js
import 'App.css';
import useCounter from 'hooks/useCounter';
import KanyeQuote from 'KanyeQuote';
import OfficeQuote from 'OfficeQuote';

function App() {
  const {counter, increment, decrement, clear} = useCounter();

  return (
    <div className="App">
      <h4>React Custom Hooks</h4>
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={clear}> 0 </button>
      <div>
        Counter = {counter}
      </div>

      <ul>
        { !(counter % 2) && <KanyeQuote />}
        { !(counter % 2) && <OfficeQuote />}
      </ul>

    </div>
  );
};

export default App;
```

---

* The objective from Compass

```js
import { useState } from 'react';

export default function useVisualMode(initial) {
  const [history, setHistory] = useState([initial]);
  const [mode, setMode] = useState(initial);

  // Purpose of transition is to put something on the history stack
  // Add the new thing to the array
  const transition = function(newMode) {

    // // Don't change state! Set state to a new one
    // history.push(newMode); // Not cool
    // setHistory(history); // Don't do it

    // // Make a newHistory that has the old history
    // // This isn't a bad solution
    // const newHistory = [...history]
    // newHistory.push(newMode)
    // setHistory(newHistory)

    // Best solution:
    setHistory(prev => {
      // Same as newHistory.push(newMode);
      const newHistory = [...prev, newMode]
      setMode(newMode);
      return newHistory;
      // Or
      // return [...prev, newMode];
    })
  };

  const back = function() {

    // // Don't mutate original
    // history.pop();
    // const newMode = history[history.length - 1];

    const copy = [...history];
    copy.pop();
    const newMode = copy[copy.length - 1];
    setHistory(copy);
    setMode(newMode);

  };

  return {mode, transition, back};

}
```