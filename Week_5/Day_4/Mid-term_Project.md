# Mid-term Project

### User Stories

* Describe how users will interact with our app
* As a ___, can I ___, because ___
  * What they want to do and the reasoning *why*
  * Convert the descriptions for the project into a user story

---

### Identify Resources

* We can then identify our resources
  * nouns === resources
    * User
    * Map
    * Restaurant
    * Pins, favourites, etc.
  * A resource is a table - table for `users`, `maps`, `restaurants`
* Build out the ERD
  * Include ERD in demonstration (maybe)

---

### Build our Routes

* We need a way to access that data
* RESTful routes
  * REpresentational State Transfer
    * The way we interact with our data represents the underlying structure of the data
    
```
-----------------------------------------
|   B   |   GET   |   /users            |
|   R   |   GET   |   /users/:id        |
|   E   |   POST  |   /users/:id        |
|   A   |   POST  |   /users/           |
|   D   |   POST  |   /users/:id/delete |
-----------------------------------------
```

   * REST is a naming convention, and all we change is the noun that's used in the routes
* Do those five things (BREAD) with each route you create

* Shows how the data structure is created
  * For example, `/maps/:map_id/pins` shows us that we have a many-to-many set up between maps and pins
  * Or, `/authors/:author_id/books` shows that `author_id` is a foreign key in `books`, but we can still have `/books/:book_id`

---

### Minimum Viable Demo

* If you're not going to show off a feature, don't build it
* Categorize your features on necessary and not necessary
  * Anything not necessary can be stretch when you have time

### Wireframes/Mockups

* Outline what our website looks like
* Using a program to develop what our page should look like
* Consider negative space / white space in design
* Steal ideas from other websites - there's a reason most websites look the same

### User Login

* Just don't, just change URL to change users

```js
app.get('login/:user_id', (req, res) => {
  // cookie-session
  req.session.user_id = req.params.user_id;

  // plaintext cookies
  res.cookie('user_id', req.params.user_id);

  res.redirect('/');
});
```