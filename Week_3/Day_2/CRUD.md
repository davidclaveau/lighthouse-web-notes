# CRUD

## Small Express Review

### Setting up the basics

* Fun fact: `localhost:8080` is generally just a developers working port.
* Don't forget that we need to create a `package.json` through an `npm init -y` and then we can install Express with `npm i express`

* Within the `server.js` file:

```js
const PORT = 8080;
const express = require("express")
const app = express(); // Don't forget to call immediately 
```

* When we create the routes with Express, we can specify the `get` request
  * The callback params are typically `req` and `res`

```js
app.get("/", (req, res) => {
  res.send("Home");
});
```

* And we can't forget to add a `listen` function within the app

```js
app.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}`)
});
```
---

## READ route

* Create a READ route for our app:

```js
app.get("/objectives", (req, res) => {
  // Knows to look in views/ directory
  // For the `objectives.ejs` file
  res.render("objectives");
});
```

* And we need to create a `views` directory in our file, and add the view (`objectives.ejs`) template.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Objectives</title>

</head>
<body>
  
</body>
</html>
```

### Templates

* A template, being mostly `html`, is a vehicle to return dynamic information to the browser
  * We can put the value of *variables* into the template
  * This allows `html` to be dynamic and change the data that's presented as required

* And we add two things:
  * We use `npm i ejs` to install the `ejs` package for templates
  * We add a `.set` to the `server.js` file

```js
const PORT = 8080;
const express = require("express")
const app = express();

app.set("viewset engine", "ejs") // <---
```

* And now we can pass variables to the template

```js
app.get("/objectives", (req, res) => {
  let awesome = 'Awesome';
  
  // We need to pass the key-value pair(s)
  // As the second argument to .render
  const templateVars = { feeling: awesome };
  
  res.render("objectives", templateVars);
});
```

* Now when we go to the `objectives.ejs` in `views/`, our file will now have the `templateVars` object to work with.
  * templateVars is traditionally a thing that is used when using templates
  * We can use the `ejs` tags to output different variables in the view

```js
// Has output in html
<%= %>

// Does NOT output
// (Like using `for` loops, or other JS code
// in the HTML file)
<% %>
```

* For example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Objectives</title>
</head>
<body>

    <h1>Objectives</h1>

    <h2><%=feeling%></h2>
    <!-- grabs templateVars.feeling and prints out 'Awesome' on the html page -->
  
</body>
</html>
```

* With our `objectives` questions variable...

```js
const objectives = {
  1: {question: "Why EJS Templates?", answer: "We use templates to separate business logic from the presentation layer."},
  2: {question: "How do we implement EJS Templates?", answer: "npm i ejs, mkdir views, app.set('view engine', 'ejs');"},
  3: {question: "What does CRUD stand for?", answer: "Create, Read, Update, Delete"},
  4: {question: "Where are URL parameters stored?", answer: "req.params"},
  5: {question: "Where are <form> values stored?", answer: "req.body"}
}
```

* We can pass the object to the `templateVars` and into the template:

```js
app.get("/objectives", (req, res) => {
  const templateVars = { objectives: objectives };
  res.render("objectives", templateVars);
});
```

* And that will print the `[Object][object]` to the html page

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Objectives</title>
</head>
<body>

    <h1>Objectives</h1>

    <table>
      <tr><th>Question</th><th>Answer</th></tr>
      
      <!-- Note, you need the alligator tag on each end of JS code-->
      <!-- Think about it like: what is this line doing? HTML or JS? -->
      <% for (let objective in objectives) { %>

        <tr><td>ROW ELEMENT 1</td><td>ROW ELEMENT 2</td></tr>
      
      <% } %>
    
    </table>
  
</body>
</html>
```

* Using the `objectives` variable, we can add aligator tags to output the variable, like so:
  * (*Remember that the `objectives` variable is numbers 1 through 5, with questions and answers within each number.*)

```html
<table>
  <tr><th>Question</th><th>Answer</th></tr>
  
  <% for (let objective in objectives) { %>

    <tr>
      <td><%= objectives[objective].question %></td>
      <td><%= objctives[objective].answer %></td>
    </tr>
  
  <% } %>

</table>
```

---

## CREATE route

* We add the `.get` in our `server.js` file to show the form

```js
app.get("/new", (req, res) => {
  res.render("new");
});
```

* We create a `new` view as well

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>New</title>
</head>
<body>
  <h1>New</h1>


<!-- We specify the path for the form -->
<!-- When we submit, the html expects an endpoint for 'action' -->
<form action="/newpath" method="POST">

  <label for="question">Question</label>
  <input id="question" type="text" name="question" />

  <label for="answer">Answer</label>
  <input id="answer" type="text" name="answer" />

  <input type="submit" name="enter" value="Add New" />

</form>

</body>
</html>
```

* And now we add the `.post` to our `server.js` file

```js
app.post("/newpath", (req, res) => {

});
```

* And we use middleware to pull in the submission details

* We use `body-parser` to take this information and make it readable for us

```js
const PORT = 8080;
const express = require("express")
const bodyParser = require("body-parser"); // <---

const app = express();
```

* And then we use the middleware on every request

```js
app.use(bodyParser.urlencoded({extended: false}));
```

* And then we can call `req.body` in the `.post` route

```js
app.post("/newpath", (req, res) => {
  console.log("req.body", req.body);
});
```

* And when we submit the form on the `new` html form, `req.body` returns the object:

```
req.body: {
  question: 'This was my question',
  answer: '42',
  enter: 'Add New'
  }
```

* And then we can create a `.redirect` in the `.post` route

```js
app.post("/newpath", (req, res) => {
  console.log("req.body", req.body);
  // Add the new objective
  res.redirect("/objectives");
});
```

```js
app.post("/newpath", (req, res) => {
  console.log("req.body", req.body);

  // Get the `current` keys from the objectives object
  const keys = Object.keys(objectives).map(x=>+x);
  // Get the (current) last element in the keys
  let last_element = key[keys.length - 1];
  // Then, get the newly added element's position, to be added
  let next_element = last_element + 1;

  // Add the form submission to that element
  objectives[next_element] = {
    question: req.body.question,
    answer: req.body.answer
  }

  res.redirect("/objectives");
});
```

## DELETE route

```js
app.get("/objectives/:id/delete", (req, res) => {
  // the ':id' is not literal, it will be replaced
  // We want to grab the `:id` off the URL
  const idToDelete = req.params.id;
});
```

* `req.params` has all of the parameters that are in your dynamic URL

```js
app.get("/objectives/:id/delete", (req, res) => {
  const idToDelete = req.params.id;
  delete objectives[idToDelete];
  res.redirect("/objectives");
});
```

* Go back to our HTML page for `objectives.ejs` view:

```html
<table>
  <tr><th>Question</th><th>Answer</th></tr>
  
  <% for (let objective in objectives) { %>

    <tr>
      <td><a href="/objectives/<%= objective %>/delete"</a></td>
      <td><%= objectives[objective].question %></td>
      <td><%= objctives[objective].answer %></td>
    </tr>
  
  <% } %>

</table>
```