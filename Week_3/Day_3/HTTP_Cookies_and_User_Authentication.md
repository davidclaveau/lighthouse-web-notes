# HTTP Cookies & User Authentication

### Quick Review from CRUD

* You may notice some URLs will look something like this:

```
www.example.com/blog/443/edit
```

* Many websites follow this
  * Path of the URL to get you to the right place (`/blog/`)
  * Number or pattern to get something specific to you (`/443/`)
    * To get the specific number, there are ways to specify that **dynamically**
    * We see with Node that we use `req.params`
      * Eg. `req.params.id` when we create the route - this corresponds to the `/blog/:id/` that we use.
      * (Conversely, we get the form data from the `req.body`)

--- 

## Cookies

* Cookies have name-value pairs
* When you first visit the website, the cookies are assigned
* When you come back to the website, the cookies are bundled and sent back to the website where they can 'recognize' your computer and identify you.
  * This can be login info, preferences, shopping cart, etc.

* HTTP Protocol is often called 'stateless'
  * The computer on the back-end is seen as being in a state (remember that the server has a certain configuration in memory) that, when that second web request comes in from an HTTP request, it has **no way of knowing anything about previous requests**.

* Cookies are not stateless, because they are able to track the history of information
  * There was a large push for the web to facilitate this kind of stuff, where now laws are only starting to catch up

---

* With rendering a page, such as `/login` or `/register`, we use `.get` route.
* To access the form on this page, we need to use a `.post` route with the same `/login` or `/route` path
  * In the template, it's the `value` attribute in the `input` tag that is seen by the user ("Save", "Register", "Delete", etc.)

```html
<input type="submit" value="Register">
```

* We use the form to capture the username and password, and push that to our database with a key-value pair of username-password

```js
users[req.body.username] = req.body.password
```

* When we editing the `.post` for the `/login` route, we want to hold the variables in **convenience variables**

```js
app.post("/login", (req, res) => {
  const testName = req.body.username;
  const testPassword = req.body.password;
});
```

* These are considered convenience variables, but also *candidate values* as we don't know if the data entered is being saved yet.
  * Helps simplify writing the next lines

```js
app.post("/login", (req, res) => {
  const testName = req.body.username;
  const testPassword = req.body.password;

  if (testPassword === users[testName]) {
    // Set a cookie, to represent that they're logged in
    res.cookie("user", testName);
    // Redirect to the homepage (or the /profile page)
    res.redirect("/");
  } else {
    // Otherwise, reload login page
    res.redirect("/login")
  }
});
```

* Now we want to create the profile page, and we can use the cookie for this

```js
app.get("/profile", (req, res) => {
  // As a key on req.cookies -> .user will get us the password
  const secretMessage = "password" + users[req.cookies.user];
  res.send(secretMessage);
});
```

* Now that we see how this works, we can create useful functionality once the user is logged in

```js
app.get("/profile", (req, res) => {
  // Check the cookie to make sure it's there
  if (users[req.cookies.user]) {
    const secretMessage = `password: ${users[req.cookies.user]}`;
    res.send(secretMessage);
    res.end();
  } else {
    res.send("Nothing to see here");
    res.end();
  }
});
```

And our `/logout` page will look like this:

```js
app.get("/logout", (req, res) => {
  res.clearCookies("user");
  res.redirect("/login");
});
```

* Of course, all of this is quite problematic because cookies are easily available to end users
  * We wouldn't want to share the cookie values as plain text
  * The code needs to be a lot more defensive about this stuff

---

* How to edit something in your app, such as a password for user login

```js
app.get("/edit", (req, res) => {
  // Grab the user from cookies
  const username = req.cookies.user;
  // Match the cookie user to our password 'database'
  const password = users[username];
  // Now we want to pass the password value to the template
  // So it can be rendered in the template in some form for the user
  const templateVars = {password: password}
  res.render("/edit", templateVars);
});

app.post("/edit", (req, res) => {
  // Now, we use `req.body` to take the password change
  // And change our 'database'
  // We could use convenience variables here:
  const username = req.cookies.user;
  const password = req.body.password
  users[username] = password
  res.render("/profile");
});
```