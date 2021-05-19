# AJAX

## Single-page Application Model

* A single-page app that's driven by AJAX calls
  * You've just downloaded everything to the page that you need to run the single-page app

* AJAX is kind of a very small browser
  * You make a call, and that call is targetted to return a *part* of the webpage (not the full page).
  * From the browser you make a call, it makes a connection somewhere on the web, and pulls in some micro-page or data
  * Just a web request that brings in some info
  * There's no evidence that this has happened for the user, unless they look at the Network tab in Chrome Dev Tools

* JS -> Calling a function -> browses the web for you -> returns data -> data is sent back to the client in a JSON format -> rendered on the page


* A multi-page app, we use `<form>` tag and request info from the browser, which refreshes the page or redirects to a different page

* What you will see in the JS is this line: `event.preventDefault();`
  * This stops the browser from submitting a form by default

## Syntax of the Ajax Request

* `.ajax` is a function call, that takes a **config object**
  * The config object has a **url** and the **method**

```js
$.ajax({
  url,
  method: 'GET',
})
  .done((data) => {
    //Getting the result

  })
  .fail(() = {
    alert('error');
  })
  .always(() = {
    console.log("Complete");
  })
```

## Pros/Cons of AJAX

### Pros
* Improves user experiences - no page reloads, faster page renders and improved response times
  * Client side rendering -> reduct network load

### Cons
* Creating dynamic content is tricky
* Async programming patterns are more complex to code
* Require JS and XMLHttpRequest support
* History is not automatically updated (no `back` or `forward` button)
  * You have to take the additional steps to support these types of functions

---

## Making AJAX Calls

* We have normal home to handle `GET` requests, and a wildcard (`*`)

```js
app.get('/', (req, res) => {
  //Homepage
});

app.get('*', (req, res) => {
  //404 errors
});
```

* And we have our HTML code, like so:

```html
  <body>
    <h1>Data Display</h1>
    <form action="/bridgeToNowhere" method="Get">
      <input id="timegetter" type="submit" name="timegetter" value="Get Data" /> ...
    </form>
    <div class="display">
      <!-- This is where we're going to stick the Ajax -->
    </div>
  </body>
```

* We aim to make the `form` tags redundant, we're going to take over for our AJAX requests.

* In our JS, we have jQuery targetting the `$("form")` and we use `event.preventDefault();` to stop the form from running and reloading the page.
  * We do this through an anonymous function call

```js
$("form").on("submit", (event) => {
  event.preventDefault(); // <-- stop that GET request
  console.log("the default event result has been prevented");
  let url = "https://api.apify.com/v2/key-value-stores/asdfasdf"

  $.ajax({
    url: url,
    method: 'GET', // POST, PUT, DELETE
  })
    .then((result) => {
      console.log('ajax callback called');
      console.log('result', result); // Grab JSON
      $('#display').html(resultTurnedIntoDOM(result));
      history.pushState(null, 'Stats Retrieve', '/#data');
    })
    .catch(err => {
      console.log('ajax error caught');
      console.log(err);
    })
});
```

* `.then` is what is processed when the AJAX call works
  * We can use the `result` to process what's returned from `url` from the `.ajax` call
  * The `result` is a piece of `JSON` that we can utilize

* We use `resultTurnedIntoDOM(result)` to turn the JSON result into something to be rendered in the HTML page:

```js
$('#display').html(resultTurnedIntoDOM(result));
```

```html
<div class="display">
  <!-- This is where we're going to stick the Ajax -->
</div>
```

* `resultTurnedIntoDOM` is a function that we create that makes a small section for the HTML page

* (We can use `$` in our variable names to help signify that we're targetting elements in the DOM)

* We can create separate `div`s and place the data and styling in them as needed

```js
function resultTurnedIntoDOM(data) {
  // This will show total infected as a div
  const $elementsForDisplay = $('<div class="canada-conid-data"></div>');
  const $infected = $('<div class="infected">Infected: ' + data.indected + '</div>');
  $elementsForDisplay.append($infected);

  // Create a table with the JSON data
  const setOfRegions = data.infectedByRegion
  // String literals to create the table
  // Create a `$regions` DOM element
  const $regions = $(`
  <table class="region">
    <tr>
      <th>Region</th>
      <th>Infected</th>
      <th>Deceased</th>
    </tr>
  </table>
  `);

  jQuery.each(setOfRegions, (key) => {
    // jQuery knows how to .append a new row in a table
    $regions.append(`
    <tr>
    <td>${setOfRegions[key].region}</td>
    <td>${setOfRegions[key].infectedCount}</td>
    <td>${setOfRegions[key].deceasedCount}</td>
    </tr>
    `);
  });

  // .appentTo will do the opposite of .append
  // Same as writing $elementsForDisplay.append($regions);
  $regions.appendTo($elementsForDisplay);

  return $elementsForDisplay;
}
```

---

## Updating the URL

```js
$("form").on("submit", (event) => {
  event.preventDefault();
  console.log("the default event result has been prevented");
  let url = "https://api.apify.com/v2/key-value-stores/asdfasdf"

  $.ajax({
    url: url,
    method: 'GET',
  })
    .then((result) => {
      console.log('ajax callback called');
      console.log('result', result);
      $('#display').html(resultTurnedIntoDOM(result));

      // We use this to change the history
      history.pushState(null, 'Stats Retrieve', '/#data');

    })
    .catch(err => {
      console.log('ajax error caught');
      console.log(err);
    })
});
```

--- 

## CORS

* Cross-Origin Resource Sharing

* For security reasons, browsers restrict cross-origin HTTP requests initiated form scripts

* A web application using those APIs can only request resources from the same origin the application loaded to
  * Browser is honouring a security policy of an API
  * It's the API provider that decides whether or not to deploy this extra layer of security

* We can use a browser extension to sip the CORS issues
  * Only good if you're running from your own machine, and not deployed into the real world

* The other solution is to route your request through a proxy.

* [More info in this Medium article](https://medium.com/@dtkatz/3-ways-to-fix-the-cors-error-and-how-access-control-allow-origin-works-d97d55946d9)