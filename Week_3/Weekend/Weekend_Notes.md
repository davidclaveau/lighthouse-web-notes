# What is meant by 'Stack'

* A stack often refers to the collection of technologies used in a given system

* For instance, we're using:
  * Node.js
  * Express
  * EJS

* But it also means the database and the hosting infrastructure, such as:
  * Heroku
  * MongoDB

---

## How HTML, CSS, and DOM are Related

* HTML uses tags to wrap the content of the page.
* CSS uses syntax and rules to apply stylistic changes to the content within the tags
  * We can do this by identifying class or id names
  * We can do inline styling using the `style` attribute
  * We can place styling information (usually in the `head` tag) by placing the CSS rules between `style` tags.
  * Or we can link to an external style sheet (preferred way)
* The Document Object Model (DOM) is how the browser reads the data, as nodes and objects

---

## IDs vs Classes

* Generally speaking, `id`s should be used to identify unique elements on the page, whereas `class`es should be used for content of the same type.
  * Targetting an `id` with CSS styling should target 0 to 1 elements
  * Targetting a `class` with CSS styling should target 0 to n elements

### Classes

* `Class`es imply stylistic or behavioral properties about an element
  * For example,
```html
<nav class="secondary disabled">
```
* describes a **navigational element** that is **secondary** and for whatever reason is also **disabled** at this time.

* Classes should generally be much more used than `id`s

### IDs

* IDs may be used when you have a unique element such as a call to action that is styled and/or behaves very differently than other elements on a page or website.

* Usually, `id`s should only be used once on each page

* IDs also need be used when you need to reference them from the URL using the anchor hash value
  * Such as jumping the comment section on the page:

```html
http://yourdomain.com#comments
```

* Some devs *may* argue we should use `id`s more liberally, as it runs faster when targetting (don't need to loop through all other instances like with `class`es)
  * However, performance can sometimes be at odds with writing *clean* and *modular* code that's *maintainable*

---

## Event Driven Architecture (EDA)


* Event Driven Architecture can be summarized as:

> When X happens, then do Y

* X is described as the *event* and Y is the *event handler*

### Client-Side Events

* We use this with the DOM which as events like `onClick`, `onLoad`, and `onFocus`, as well as custom events

### Server-Side Events

* A server running Node.js, think of the incoming request as an event, with a callback function that handles the event.

* The Node.js core API provides an `EventEmitter` class that is basis for event-driven patterns.

---


## What is the DOM

* The document object model (DOM) is an interface that allows a programming language to manipulate the content, structure, and style of a website. 
  * JavaScript is the client-side scripting language that connects to the DOM in an internet browser.

* The document object is a built-in **object** that has many **properties** and **methods** that we can use to access and modify websites.

* It may see that the HTML source code and DOM are the same thing, but there are two instances in which the browser-generated DOM will be different than HTML source code:
  * The DOM is modified by client-side JavaScript
  * The browser automatically fixes errors in the source code

* In an HTML document, we can target **properties** (or tags) of the document

  * Source code:
```html
#document
<!DOCTYPE html>
<html lang="en">

  <head>
    <title>Learning the DOM</title>
  </head>

  <body>
    <h1>Document Object Model</h1>
  </body>

</html>
```

* When we call `document.body` we get:

```html
<body>
  <h1>Document Object Model</h1>
</body>
```

* `document` is an object, `body` is a *property* of that object that we have accessed with dot notation. Submitting document.body to the console outputs the body element and everything inside of it.

---

## Event Propagation

* Since DOM events are nested within other elements, in a tree-like structure, events that affect a child element *bubble up* through its parents.

* Bubbling:
  * When an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors.

* `event.target` on a parent element can indicate which nested element was *targeted*
  * ``event.target`` – is the “target” element that initiated the event, it doesn’t change through the bubbling process.
  * `this` – is the “current” element, the one that has a currently running handler on it.

* For example, with HTML:

```html
<form id="form">FORM
    <div>DIV
      <p>P</p>
    </div>
  </form>
```

* And JS:

```js
form.onclick = function(event) {
  event.target.style.backgroundColor = 'yellow';

  // chrome needs some time to paint yellow
  setTimeout(() => {
    alert("target = " + event.target.tagName + ", this=" + this.tagName);
    event.target.style.backgroundColor = ''
  }, 0);
};
```

* This alert will say which nested tag was the "target" of the click (`p`, `div` or `form`), whereas `this` will always be the `form` tag.

* We can use `event.stopPropagation()` to stop bubbling from occurring during events that are completed.
  * If an element has multiple event handlers on a single event, then even if one of them stops the bubbling, **the other ones still execute**.
  * In other words, `event.stopPropagation()` stops the move upwards, but on the current element all other handlers will run.
  * To stop the bubbling and prevent handlers on the current element from running, there’s a method `event.stopImmediatePropagation()`. After it no other handlers execute.

* There's another event called **capturing** that's rarely used

* Capturing is the inverse of bubbling, in that an event that has a target will pass through the ancestors chain down to the element (*capturing phase*), then it reaches the target and triggers there (*target phase*), and then it goes up (*bubbling phase*), calling handlers on its way.

* Handlers using `on<Event>` don't know anything about capturing, as they're only run on target and bubbling phases.

* To catch an event on the capturing phase, we can run an additional option in the handler to `true`:

```js
elem.addEventListener(..., {capture: true})
// or, just "true" is an alias to {capture: true}
elem.addEventListener(..., true)
```

* There are two possible values of the capture option:

  * If it’s false (default), then the handler is set on the bubbling phase.
  * If it’s true, then the handler is set on the capturing phase.