# React

## Virtual DOM

* Virtual DOM is a memory copy of the browser's contents. If something changes in the browser's contents, Reacht will find and make the changes of the differences.

* That's what makes React's changes, transitions and animations so smooth - they changes are in memory and React builds a "patch" to change the parts on the screen (as opposed to re-rendering the whole page).

## Class-based vs Functional Components

* Not something that we will do in our program, as class-based is a little out of date and not as easy to use.

* The modern way is to use **functional components** and it's recommended to use functional components where you can.

---

## Create React App

* Most people would download the React library and start by using the Create React App command
  * `npx` allows you to complete an executable without downloading the library
  * `npx create-react-app [app name]` will create a new React app and download the React scripts and run the create app script - install everything we need.
  * Downloads the hooks, react modules, and webpacks - a complete development environment
    * Webpack is like a server, it has scripts that helps with running and refreshing the *view* the page when we make changes.
  * We also get *Babel* in the webpack. This helps with translating JSX into what the browser can understand.
  
* React only runs on the browser and only affects the view of the app.

* In the `public` directory, we only really care about the `index.html` file; in the `src` directory, we similarly only care about `App.js`, `App.css`, `index.js` and `index.css`
  * We can remove the other files to clean it up for now.

---

## React Files

* In our `index.html` file, we have the `div id="root"></div>` - and that's where our React app will live.

* `index.js` will let us access the index.html and target the root id.

* We use `import React from 'react';` to bring in React (we don't use `require` as that's a Node app - this is for the browser)

We have `ReactDOM.render(<App />, document.getElementById('root'))` to target the root div.
  * Takes two parameters - the components in a `js` file and the element in `index.html` that it's accessing.

---

## Anatomy of a Component

* Every React component is a JS function

* Two major parts of a component - you have all things that you're getting ready (the functionality of the component, the scripts) and the `return` that we use to render the component.

---

## JSX

* JSX is the merging of JS and HTML.

* First rule of JSX - we can't use the word 'class'

* We need curly brackets when we integrate JS into the render part of JSX

```js
const hello = "hello world";

return (
  <div className="App">
    <button>{hello}</button> // <--variable wrapped in curly braces 
  </div>
);
```

* We can also take the `<button>` and build it outside of the `return` (although it's a weird choice in this instance)

```js
const button = <button>Hello World</button>

return (
  <div className="App">
    {button}
  </div>
);
```

* To get the button to be userful and interactive, we need to create a `onClick` function for the button

```js
const buttonText = "Click me!"

const myButtonClick = function() {
  console.log("Button clicked!");
};
 
return (
  <div className="App">
    <button onClick={myButtonClick}>{buttonText}</button>
  </div>
);
```

* We can add more dynamicism to our button

```js
const counter = 0;
const buttonText = "Click me!"

const myButtonClick = function() {
  console.log("Button clicked!");
};
 
return (
  <div className="App">
    <button onClick={myButtonClick}>{buttonText}</button>
  </div>
  <div>
    Counter = {counter}
  </div>
);
```

* But we'll get an error because our JSX cannot return separate `divs` or parents - you need to return **one single parent**

```js
let counter = 0;
const buttonText = "Click me!"

const myButtonClick = function() {
  counter++;
  console.log("Button clicked!", counter);
};
 
return (
  <div> // <--- single parent tag
    <div className="App">
      <button onClick={myButtonClick}>{buttonText}</button>
    </div>
    <div>
      Counter = {counter}
    </div>
  </div>
);
```

* But that's a lot of `div`s, so we should use a `Fragment` instead
  * This can be done with simple empty tags `<></>`

* If we check our `counter`, the counter is changing and going up, but *not rendering*
  * The main problem is that our counter isn't re-rendering
  * There will only be a change if there's a **change in state** or a **change in props**

* We need a variable to survive a re-render
  * We need to utilize `useState` to accomplish this

```js
// Create the useState, set the initial state of counter to 0
const [counter, setCounter] = useState(0);

const buttonText = "Click me!"

const myButtonClick = function() {
  setCounter(counter + 1);
  console.log("Button clicked!", counter);
};
 
return (
  <div>
    <div className="App">
      <button onClick={myButtonClick}>{buttonText}</button>
    </div>
    <div>
      Counter = {counter}
    </div>
  </div>
);
```

* `useState` returns an array

* `useState` takes two parameters

* Saves the last value that was used in memory in the React app

* It's the same as writing this:

```js
const array = useState();
const value = array[0];
const function = array[1];
```

---

### List

* We can add a list

```js
const [counter, setCounter] = useState(0);

const buttonText = "Click me!"

const myButtonClick = function() {
  setCounter(counter + 1);
  console.log("Button clicked!", counter);
};

const list = [
  <li>item 1</li>,
  <li>item 2</li>,
  <li>item 3</li>,
  <li>item 4</li>
];
 
return (
  <div>
    <div className="App">
      <button onClick={myButtonClick}>{buttonText}</button>
    </div>
    <div>
      Counter = {counter}
    </div>

    <ul>
      {list} // <-- use our list variable
    </ul>

  </div>
);
```

* This called a **controlled component**
  * The component is *controlled by the data that it gets*

---

### List with outside Data

* Pretend that we have data coming in from an outside source, as an array of objects

```js

//Data
const data = [
  {id: 1, quote: "quote"},
  {id: 2, quote: "other quote"}
];

function App() {

  const [counter, setCounter] = useState(0);

  const buttonText = "Click me!"

  const myButtonClick = function() {
    setCounter(counter + 1);
    console.log("Button clicked!", counter);
  };

  const list = [
    <li>item 1</li>,
    <li>item 2</li>,
    <li>item 3</li>,
    <li>item 4</li>
  ];
  
  return (
    <div>
      <div className="App">
        <button onClick={myButtonClick}>{buttonText}</button>
      </div>
      <div>
        Counter = {counter}
      </div>

      <ul>
        {list} // <-- use our list variable
      </ul>

    </div>
  );
};
```

* And we can use the `.map()` function to get the items for the array

```js

//Data
const data = [
  {id: 1, quote: "quote"},
  {id: 2, quote: "other quote"}
];

function App() {

  const [counter, setCounter] = useState(0);

  const buttonText = "Click me!"

  const myButtonClick = function() {
    setCounter(counter + 1);
    console.log("Button clicked!", counter);
  };

  // And we can use .map() to get the items into the list
  const list = data.map(item => {
    return <li>{item.quote}</li>;
  });
  
  return (
    <div>
      <div className="App">
        <button onClick={myButtonClick}>{buttonText}</button>
      </div>
      <div>
        Counter = {counter}
      </div>

      <ul>
        {list} // <-- use our list variable
      </ul>

    </div>
  );
};
```

* Now we need to create another state to keep track of our quotes

```js

//Data
const data = [
  {id: 1, quote: "quote"},
  {id: 2, quote: "other quote"}
];

function App() {

  const [counter, setCounter] = useState(0);
  const [quoteData, setQuoteData] = useState([]) // <-- set the quote data

  const buttonText = "Click me!"

  const myButtonClick = function() {
    setCounter(counter + 1);
    setQuoteData(data); // <-- change the state of the quotes
  };

  // Put the "star of the show" right before the return
  const list = quoteData.map(item => {
    return <li>{item.quote}</li>;
  });
  
  return (
    <div>
      <div className="App">
        <button onClick={myButtonClick}>{buttonText}</button>
      </div>
      <div>
        Counter = {counter}
      </div>

      <ul>
        {list} // <-- use our list variable
      </ul>

    </div>
  );
};
```

* You will also get the error `each child in a list should have a unique "key" prop`
  * React wants to know what to render and what it doesn't need to render again
  * We can add that to our list

```js
  const list = data.map(item => {
    return <li key={item.id}>{item.quote}</li>; // <-- add key
  });
```

---

### Input

* Now we can use `<input>` to take some text

```js

//Data
const data = [
  {id: 1, quote: "quote"},
  {id: 2, quote: "other quote"}
];

function App() {

  const [counter, setCounter] = useState(0);
  const [quoteData, setQuoteData] = useState([]);
  const [input, setInput] = useState(""); // <-- input state

  const buttonText = "Click me!"

  const myButtonClick = function() {
    setCounter(counter + 1);
    setQuoteData(data);
  };

  const list = quoteData.map(item => {
    return <li>{item.quote}</li>;
  });
  
  return (
    <div>
      <div className="App">
        <input type="text" value={input}/> // <-- add an input
        <button onClick={myButtonClick}>{buttonText}</button>
      </div>
      <div>
        Counter = {counter}
      </div>

      <ul>
        {list}
      </ul>

    </div>
  );
};
```

* React will say `a component is changing an uncontrolled input to be controlled... Possibly changing from undefined to a defined value, which should not happen.`

* So we should give the `input` a function

```js

//Data
const data = [
  {id: 1, quote: "quote"},
  {id: 2, quote: "other quote"}
];

function App() {

  const [counter, setCounter] = useState(0);
  const [quoteData, setQuoteData] = useState([]);
  const [input, setInput] = useState("");

  const buttonText = "Click me!"

  const myButtonClick = function() {
    setCounter(counter + 1);
    setQuoteData(data);
  };

  const onChange = function (event) {
    setInput(event.target.value); 
  };

  const list = quoteData.map(item => {
    return <li>{item.quote}</li>;
  });
  
  return (
    <div>
      <div className="App">
        <input type="text" value={input} onChange={onChange} /> // <-- add a function to track the changes in input
        <button onClick={myButtonClick}>{buttonText}</button>
      </div>
      <div>
        Counter = {counter}
      </div>

      <ul>
        {list}
      </ul>

    </div>
  );
};
```

* Needs a place to put the `input` value (`value={input}`)

* Needs a way to change the value (`onChange` function)

---

### Props

* A component is a function that returns JSX

* So we can create a new component called `Button` that returns a `<button>` tag

```js
const Button = function(props) {

  return (
    <button></button>
  );

};
```

* What we can then do in our `App` component is call the `<Button />` and set some attributes to be used as props

```js
return (
  ...
  <Button text="Hello from App" />
  ...
)
```

```js
const Button = function(props) {

  return (
    <button>{props.text}</button>
  );
};
```

* We always have `props.children` in our props.

* Everything inside the component is considered the `children`

```js
return (
  ...
  <Button text="Hello from App">These are the Children</Button>
  ...
)
```

* We can also pass functions as props into the component. For example, passing our `myButtonClick` as an attribute to be used as a prop

```js
return (
  ...
  <Button clicker={myButtonClick} text="Hello from App" />
  ...
)
```
```js
const Button = function(props) {

  return (
    <button onClick={props.clicker}>{props.children}</button>
  );
};
```