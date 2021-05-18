# DOM Events

## Adding Event Listeners

* We can think of DOM events quite simply as:

```js
document.addEventListener(<string>, <callback>, <use-capture>)
```

* `string` is any of the events, like "click", "mousedown", or "touch start"
* `callback` is the function that's called when the event happens, and that event is passed as the first argument
* `use-capture` is a boolean that says whether the callback should be called in the "capture phase" or not.

## Removing Event Listeners

* It's considered good practice to remove event listeners when they are no longer needed using the `element.removeEventListener()` method

```js
element.removeEventListener(<event-name>, <callback>, <use-capture>);
```

* However, ou must have a reference to the callback function that was originally bound. Simply calling `element.removeEventListener(‘click’)`; will not work.

* Essentially, if we have any interest in removing event listeners, then we need to keep a handle on our callbacks. This means we **cannot use anonymous functions**.

## The Event Object

* The event object is created when the event first happens. The function that we assign as a callback to an event listener is passed the event object as its first argument. We can use this object to access a wealth of information about the event that has occurred:
  * `type` (string) This is the name of the event.
  * `target` (node) This is the DOM node where the event originated.
  * `currentTarget` (node) This is the DOM node that the event callback is currently firing on.
  * `bubbles` (boolean) This indicates whether this is a “bubbling” event.
  * `preventDefault` (function) This prevents any default behaviour from occurring that the user agent (i.e. browser) might carry out in relation to the event 
    * For example, preventing a click event on an `<a>` element from loading a new page.

* All of these properties in an event object are dependent on the event type that's passed in, such as mouse events for `click`

## Delegate Events

* Delegate event listeners are a more convenient and performant way to listen for events on a large number of DOM nodes using a single event listener.

* For example, a list contains 100 items that all need to respond to a click event. We could query the DOM for all of the list items and attach an event listener to each one.
  But this would result in 100 separate event listeners.

* Delegate event listeners can make our lives a lot easier. Instead of listening for the click event on each element, we listen for it on the parent `<ul>` element. When an `<li>` is clicked, then the event bubbles up to the `<ul>`, triggering the callback.

* If you’re using jQuery, you can seamlessly use event delegation by passing a selector as the second parameter to the `.on()` method.

## Useful Events

### `load`

* The load event fires on any resource that has finished loading (including any dependent resources). This could be an image, style sheet, script, video, audio file, document or window.

```js
image.addEventListener('load', function(event) {
  image.classList.add('has-loaded');
});
```

### `onbeforeunload`

* `window.onbeforeunload` enables developers to ask the user to confirm that they want to leave the page. This can be useful in applications that require the user to save changes that would get lost if the browser’s tab were to be accidentally closed.

```js
window.onbeforeunload = function() {
  if (textarea.value != textarea.defaultValue) {
    return 'Do you want to leave the page and discard changes?';
  }
};
```

* Note that assigning an `onbeforeunload` handler prevents the browser from caching the page, thus making return visits a lot slower. Also, `onbeforeunload` handlers **must be synchronous.**

### `resize`

* Listening to the `resize` event on the window object is super-useful for complex responsive layouts. When the window is resized or the device’s orientation changes, then we would likely need to readjust these sizes.

```js
window.addEventListener('resize', function() {
  // update the layout
});
```