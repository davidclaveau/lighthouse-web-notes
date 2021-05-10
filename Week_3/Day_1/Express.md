# Express

* Fast, unopinionated, minimalist web framework for Node.js
  * Opinionated means there are very specific rules that you have to use when using this framework - Express doesn't have this
  * Unopinionated means more customizability

* We want to call `Express` and then immediately invoke it.

```js
const express = require('express');
const app = express();
const port = 3000;

app.listen(port, () => {
  console.log(`Server is listening on port ${port}`);
});
```

* We can take that server call and apply specific functions on them

```js
const express = require('express');
const app = express();
const port = 3000;

// GET /users
app.get('/users', (req,res) => {
  // We bundle `write` and `end` into one line:
  res.send('Hello world');
});

app.listen(port, () => {
  console.log(`Server is listening on port ${port}`);
});
```

* We can `use` middleware in our Express application to help parse information in the middle of our application.

```js
const express = require('express');
const app = express();
const port = 3000;

// Middleware will always have `use`
// `.use` is always a callback
app.use((req, res) => {
  console.log(req.method);
  console.log(req.url);
});

app.get('/users', (req,res) => {
  res.send('Hello world');
});

app.listen(port, () => {
  console.log(`Server is listening on port ${port}`);
});
```

* If we run this, the Network tab will show as *pending*

* We're stuck in the `.use` method

* Every piece of middleware **needs to use** `next`

```js
app.use((req, res, next) => { // Next is a function
  console.log(req.method);
  console.log(req.url);

  next();
});
```

* Express works from top to bottom, and will do `.use` first

* We can use `morgan` inside of our server to help with logging information to the console
  * the `dev` component of morgan will give us request, status code, request time, and other useful information

```js
const express = require('express');
const express = require('morgan');
const app = express();
const port = 3000;

app.use(morgan('dev')); // <--- GET /users 200 2.644ms - -

app.use((req, res) => {
  console.log(req.method);
  console.log(req.url);
});

app.get('/users', (req,res) => {
  res.send('Hello world');
});

app.listen(port, () => {
  console.log(`Server is listening on port ${port}`);
});
```

* We can also send back JSON through our request

```js
app.get('/users', (req,res) => {
  res.json({
    message: 'hello world'
  });
});
```

* We can send back an html page as well, for example a fake `index.html` page we created:

```js
app.get('/users', (req,res) => {
  res.sendFile('./index.html') // We'll get error saying "need absolute path
});
```

* Because we need the user's directory, we can call the variable `__dirpath` to reference that user's current path.

```js
app.get('/users', (req,res) => {
  res.sendFile(`${__dirname}/index.html`) // We can use the `__dirpath` for this
});
```

* For configuring templates, we can use `.set`

```js
// Tell express to use ejs
app.set('view engine', 'ejs');
```

* We're going to use server-side rendering

```js
app.get('/users', (req,res) => {
  res.render('users', {});
});
```

* Express will try, but will get an error saying "couldn't find `users` in the *views* directory"

* This means we need to make a **views** directory

* We create a directory called `views` and a file called `users.ejs`

* In the HTML file for users, we can add the information to be rendered.

* We need to pass in the template variables as an object for the template

```js
app.get('/users', (req,res) => {

  const templateVars = {
    name: 'David',
    friend: 'Catie',
    food: 'cake',
    verb: 'snowboarding'
  };
  
  res.render('users', templateVars);
});
```

