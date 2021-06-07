#  React

## Function Components

* Conceptually, components are like JS functions. They accept arbitraty inputs (called "`props`") and `return` React elements describing what should appear on the screen.

```js
function Tweet(props) {
  return (
    <article className="tweet">
      <header className="tweet__header">
        <img className="tweet__header-avatar" src={props.avatar} />
        <h2 className="tweet__header-name">{props.name}</h2>
      </header>
      <main className="tweet__content">
        <p>{props.content}</p>
      </main>
      <footer className="tweet__footer">{props.date}</footer>
    </article>
  );
}
```

### Class Components 

* An alternative is to declare our React components as ES6 Classes. They must extend `React.Component` and overwrite the `render()` method. The `render()` method can return React elements.

## JSX

* JSX is the language that we use with React to declare what the user interface should look like. It's not required to use JSX with React, but there are many reasons why we choose to use JSX with React.

* JSX:

```js
const element = <h2 className="name">Name</h2>
```

* This code is run through a tool that knows how to turn JSX into JavaScript. The tool produces a JavaScript expression that creates a React element.

* JavaScript:

```js
const element = React.createElement("h2", {
  className: "name"
}, "Name")
```

* The `React.createElement(type, [props], [...children])` function allows us to create a hierarchy of DOM nodes.

* React requires that **we always start component names with a capital letter**. When JSX is being converted, it uses the case of the tag name to determine if we are describing a component or an HTML element.

### ReactDOM.render

* An element can be "rendered" into any DOM node using the react-dom library. This example requires `<div id="root"></div> to be declared in the HTML.

### Expressions in JSX

* JSX looks like a template language, but it's purely JS. This means it can include expressions with the JSX.
  * For instance, we can pass in the `format` function

```js
import { format } from "date-fns";

function Footer(props) {
  return (
    <footer className="tweet__footer">
      {format(props.date, "MMMM Do, YYYY")}
    </footer>
  )
}

ReactDOM.render(
  <Footer
    date="2013-05-29"
  />,
  document.getElementById("root")
);
```

## Basic React Patterns

### Children

* When a prop has children, we can access them using `props.children`

```js
function Button(props) {
  return <button>{ props.children }</button>;
}

function Application(props) {
  return ( 
  <main>
    <Button>Reset</Button> // <-- Call the Button function
  </main>
  );
}
```

### Event Handling

* We can attach handlers to React elements and pass a reference to a function directly

```js
function Button(props) {
  return (
    <button onClick={(event) => console.log("Button Clicked")}>
      {props.children}
    </button>
  );
}
```

* In some cases, we would pass a function reference similar to the example below.  This technique is userful if our **function requires multiple expressions on multiple lines**.

```js
function Button(props) {
  function onClicked(event) {
    console.log("Button Clicked");
  };

  return <button onClick={onClicked}>{props.children}</button>;
}
```

* We can pass the `onClick` function to our `<button>` element, but it's not very configurable, so we should move the `onClick` event handler to the `<Button>` component.

```js
function Application(props) {
  return (
    <main>
      <Button onClick={(event) => console.log("Button Clicked from Application")}>
        Submit
      </Button>
    </main>
  );
}
```

* But the `<button>` element needs to have a way to handle the `onClick` event that it's being passed. We therefore add the `props.onClick` to accept the event handler

```js
return <button onClick={props.onClick}>{props.children}</button>;
```

### Managing State

* State allows us to retain information across multiple renders. To store state, we need to use a feature of React called **hooks**. We specifically use the `useState` hook

* The `useState` function receives an initial state as an argument and return an array. The first element of the array is the current value for the state. The second element is a function that can update the state and cause a render.

### Controlled Components

* Certain HTML form elements maintain tehir state. An `<input>` element keeps track of what the current value is. The same is true for `textarea` and `select`. It is preferable for React to manage all of the application state including what is currently typed into an `<input>`
  * These are called **controlled components**.

* For example, the `<input>` element becomes a controlled component when we provide a value prop and an onChange event handler that can update the value.

```js
function Application(props) {
  const [name, setName] = useState("");

  return (
    <main>
      <input
        value={name}
        onChange={(event) => setName(event.target.value)}
        placeholder="Please enter your name"
      />
      <h1>Hello, {name}.</h1>
    </main>
  );
}
```

### Conditional Rendering

* Sometimes we want to render something only when it's necessary - the example above doesn't need to show "Hello ___" when no name is entered

* We can use a condition to decide which React element is returned using JSX

```js
function Application(props) {
  const [name, setName] = useState("");

  return (
    <main>
      <input
        value={name}
        onChange={(event) => setName(event.target.value)}
        placeholder="Please enter your name"
      />
      { name && <h1>Hello, {name}.</h1> } // <-- only run if name exists
    </main>
  );
}
```

### Fragments

* When a user types a name, it will render it below the input field.  We would also like to provide an easy way for the user to reset the contents of the input field. We are already using a "controlled component". Calling `setName("")` will reset the input field contents.

```js
function Application(props) {
  const [name, setName] = useState("");

  function reset() {
    setName("");
  }

  return (
    <main>
      <input
        value={name}
        onChange={(event) => setName(event.target.value)}
        placeholder="Please enter your name"
      />
      {name && (
          <h1>Hello, {name}.</h1>
          <Button onClick={reset}>Reset</Button>
      )}
    </main>
  );
}
```

* However this will cause an error in syntax. This is becasuse **components can only return one element**. The same is true wen we are evaluating expressions conditionally. We must ensure that our expression only has one React element at the root. The solution provided by the library is called a React.Fragment.

```js
{name && (
  <Fragment>
    <h1>Hello, {name}.</h1>
    <Button onClick={reset}>Reset</Button>
  </Fragment>
)}
```

## CSS Styling

* There are a few ways to style using React

### Inline styling

* The styling is in the document and referenced in React components

```js
import React from 'react';

const divStyle = {
  margin: '40px',
  border: '5px solid pink'
};
const pStyle = {
  fontSize: '15px',
  textAlign: 'center'
};

const Box = () => (
  <div style={divStyle}>
    <p style={pStyle}>Get started with inline style</p>
  </div>
);
```

### CSS Sylesheets
   * Similar to regular HTML and CSS styling, uses an external style sheet and `className` to style the element

```js
const DottedBox = () => (
  <div className="DottedBox">
    <p className="DottedBox_content">Get started with CSS styling</p>
  </div>
);
```

* and CSS

```css
.DottedBox {
  margin: 40px;
  border: 5px dotted pink;
}

.DottedBox_content {
  font-size: 15px;
  text-align: center;
}
```
  
### CSS modules 

* Which is the same as stylesheets but with extra steps, but scoped locally

```js
import React from 'react';
import styles from './DashedBox.css';

const DashedBox = () => (
  <div className={styles.container}>
    <p className={styles.content}>Get started with CSS Modules style</p>
  </div>
);
```

* and CSS

```css
:local(.container) {
   margin: 40px;
   border: 5px dashed pink;
 }
 :local(.content) {
   font-size: 15px;
   text-align: center;
 }
```

### Styled-components

* Style-components is a library for React and React Native that allows you to use component-level styles in your application that are written with a mixture of JavaScript and CSS

```js
import React from 'react';
import styled from 'styled-components';

const Div = styled.div`
  margin: 40px;
  border: 5px outset pink;

  &:hover {
   background-color: yellow;
 }
`;

const Paragraph = styled.p`
  font-size: 15px;
  text-align: center;
`;

const OutsetBox = () => (
  <Div>
    <Paragraph>Get started with styled-components 💅</Paragraph>
  </Div>
);

export default OutsetBox;
```

### BEM

* Block Element Modifier is a naming convention for all of the component styles. Consider the `DayListItem` for our app. When selected, it requires a white background, but when it is full it needs a lower opacity.

* **Block**: Encapsulates a standalone entity that is meaningful on its own. In our case this is the `.day-list`
*  **Element**: An element that is tied to its block. In our case this is the `.day-list__item`
*  **Modifier**: A modification to a block or an element. In our case, this is the `.day-list__item--selected` or `.day-list__item---full`

* We use combinations of these classes to define the different visual states an element can have.

* The specificity is enforced through *naming* rather than complex selectors.