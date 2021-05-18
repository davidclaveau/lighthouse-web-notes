# Client-side JavaScript and jQuery

## Building Tic-Tac-Toe Game in Browser

* We can easily build a tic-tac-toe board with JS/jQuery

```html
<html>
  <head>
    <title>Tic-Tac-Toe</title>
    <!-- CSS stylesheet -->
    <link rel="stylesheet" href="./tictactoe.css">
    <!-- JS sources, such as jQuery to -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <!-- Our JS script we're using -->
    <script src="tictactoe.js"></script>
  </head>
  <body>
    <table>
      <tr><td></td><td></td><td></td></tr>
      <tr><td></td><td></td><td></td></tr>
      <tr><td></td><td></td><td></td></tr>
    </table>
  </body>
</html>
```

* And give some styling in a CSS file

```css
td {
  width: 120px;
  height: 120px;
  background-color: green;
  border: 1px solid black;
}
```

* And finally create some JS/jQuery to accompany the page

```js
// (jQuery to identify the document)
$("document").ready(function() {
  // STEP THREE: is there a winner yet?

  // STEP TWO: toggle which player clicks next

  // STEP ONE: on each turn, this event handler will run
});
```

* Break down the problem into smaller problems, bit by bit
  * For example, find a way to place X's on the page
  * Then work your way to which player clicks next
  * Then find the winner and how to announce that

* There are some objects that are identified for the browser that you cannot see.
  * `document` is one of these objects

* We can use the Content Delivery System (CDN) to bring in the `jQuery` source code to use
  * Place this in the `<head>` of our HTML document.

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
```

* The `$` within our jQuery code grabs the whole `document` to read

```js
$("document").ready(function() {
});
```

* This is an **event listener** to create a **callback** when the document is interacted with.
  * It will call that anonymous function when the document has loaded (`.ready`)

---

* What we would want to do first is grab the name of the element using the CSS selector

* Then we want to specify what we want to do to that selector
  * We want to add the `.click`, which is a callback, to create an action when the class is clicked
  * We can say when the element is clicked, `addClasss` and add the `X` class

```js
$("td").click(function() {
  $(this).addClass("X");
});
```

* And we make sure to add the class to our CSS:

```css
td {
  width: 120px;
  height: 120px;
  background-color: green;
  border: 1px solid black;
}

.X {
  background-color: blue;
}
```

* Now, when it's clicked in the browser, the `td` adds the `X` class and turns blue.

---

* Now we want to ensure that our players are alternating
  * "It's X's turn" or "It's O's turn"

* We can add the text to the HTML and apply an ID so we can determine whose turn it is.

```html
<body>
  It's <span id="next">X</span>'s turn.
```

* Back to the jQuery file, we can add the functionality for when the `td` element is clicked, we look to that ID `next`
  * We can use `.html` to get the content of that element
    * Empty function call, without arguments, will **get** the content
    * Something like `.html("string")` will **replace** the content
  * We can set the `next` targetter as a variable, and then pass that variable to the `$(this).addClass` line:

```js
$("td").click(function() {
  const next = $("#next").html();
  $(this).addClass(next);
});
```

* This allows us to also use the `next` variable for conditional statments
  * So, we get the current player's turn
  * We assign the class according to the player
  * We then change the HTML depending on what `next` is, to alternate the players

```js
$("td").click(function() {
  const next = $("#next").html();
  $(this).addClass(next);
  if ('X' === next) {
    $("#next").html('O');
  } else {
    $("#next").html('X');
  }
});
```

* And we add the CSS class for our `O` player

```css
.O {
  background-color: red;
}
```

---

* However, we see that `O`'s class actually has more power in the browser.
  * `O` can click on `X`'s squares to replace the colour.
  * This is because `O`'s class comes at the end of our CSS file, so it trumps the `X` class

```css
.X {
  background-color: blue;
}
.O {
  background-color: red;
}
```

* One thing we can do is `.unbind` the `click` from the `td` element, so the event listener won't listen for that specific `td` element.
  * This will ensure any future clicks on an already-clicked `td` won't register in the browser

```js
$("td").click(function() {
  const next = $("#next").html();
  $(this).addClass(next);
  if ('X' === next) {
    $("#next").html('O');
  } else {
    $("#next").html('X');
  }
  $(this).unbind("click"); // <---
});
```

---

## Chrome Dev Tools - Console

* We can use the `console` window in the browser to look for JS and other DOM elements
  * For example, we can type `document` and the dev tools will highlight the `document`
  * We can see what `navigator` corresponds to, and what methods are attached to it
  * Ultimately, this showcases what's available to us in JS and what we can implement in the browser with JS
  * We can use `$("td")` to give us an object of all the `td` tags.

* The DOM in the Chrome Dev Tools will always change dynamically with what's clicked on the page, whereas the `view page source` will always show what was loaded initially
  * Use these to compare changes.