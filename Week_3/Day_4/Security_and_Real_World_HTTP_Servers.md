# Security and Real World HTTP Servers

* How to avoid vulnerabilities and hacking

## Security

* Passwords that are stored in plain text represent a security breach
  * The solution for this is to use **hashing**

* Hashing is *unidirectional*, so you can't run it backwards to find the source word easily - or it would take unreal computing power. 

* Each iteration is doubling the time it takes to hash the content

* We simply need to increase the iteration count if computers become more powerful

```
salt | password --> iteration count (n) hash function --> salt | hash resilt
```

* Cookie information is stored in plain-text
  * Plain text cookies can be modified
  * We might impersonate another user
  * The solution for this is to use **encryption**

### Encryption

* Information is encrypted using a key
* It can be decrypted by the recipient using the key
* A *private* key and a *public*
  * Private key you keep secret

#### Man in the Middle Attack

* HTTP is plain-text
* HTTPS is encrypted
* With HTTP, the attacker can see plain-text information that's being sent from the user's computer (being sent to the gateway/server).
* This creates end-to-end encryption

### Hashing

* bcrypt uses a callback to call the hashing function
  * We can specify the number of times we iterate over the hash

```js
const bcrypt = require('bcrypt') // Help with salting needs

const plaintextPassword = abcd

bcrypt.genSalt(10, (err, salt) => {
  bcrypt.hash(plaintestPassword, salt, (err, hash) => {
    console.log('hash', hash);
  });
});

// You will get several dozen random characters
```

* We ask, is this hash output *compatible* with the original?
  * Not asking is it *identical*

* The salt and the hash will be quite similar in output

* Using promises:

```js
bcrypt.genSalt()
  .then((salt) => {
    console.log('salt:', salt);
    return bcrypt.hash(plaintextPassword, salt);
  })
  .then((hash) =. {
    console.log('hash', hash);
  });

// Salt: $2b$10$3veVuGZyKTBH.
// Hash: $2b$10$3veVuGZyKTBH.asdfasdfasdf.asdfasdffasdf.
```

* How to check if the new hash checks out with

```js
// What the user passes in
const hash = $2b$10$3veVuGZyKTBH.asdfasdfasdf.asdfasdffasdf

bcrypt.compare('abcd', hash)
  .then((result) => {
    console.log('do the passwords match?', result);
  })

// Should return 'true'
```

### Encrypting Cookies

* We can encrypt our cookies using `cookieSession`

* `cookieSession` uses one key or another to double the difficulty for hacking your encryption key.
  * The system can use multiple keys

```js
const cookieSession = require('cookie-session');

app.use(cookieSession({
  name: 'cookiemonster',
  keys: ['my secret key', 'yet another secret key']
}));
```

* Logging in with the username and password
  * Comparing the hashed password

```js
app.patch('/login', (req, res) => {
  const username = req.body.username;
  const password = req.body.password;

  // Does the user exist?
  const user = users[username];
  if (!user) {
    return res.status(401).send('No user with that username found');
  }

  // User exists, let's compare the password given in the form
  // Against the password in the database
  // We use bcrypt.compare to compare plain-text vs hash
  bcrypt.compare(password, user.password)
    .then((result) => {
      if (result) {
        // res.cookie('username', user.username);

        // We use `cookieSessions` for decrypting the cookie
        // We assign a key-value pair
        req.session.username = user.username;
        res.redirect('/protected');
      } else {
        return res.status(401).send('Password incorrect');
      }
    });
});
```

### Hashing Passwords

* Created a hashed password when registering

```js
app.post('/register', (req, res) => {
  const username = req.body.username;
  const password = req.body.password;

  // Create the salt for the password
  bcrypt.genSalt(10)
    // Then create the hash using the salt
    // On the plain-text password
    .then((salt) => {
      return bcrypt.hash(password, salt);
    })
    // Take the hash password and store it
    // For that user in the 'database'
    .then((hash) => {
      users[username] = {
        username,
        password: hash
      };
      console.log(users);
      res.redirect('/login');
    });
});
```

---

## REST

* REpresentational State Transfer (REST) is a pattern, a convenstion to organize our URL structure

* Resources based routes convention (the key abstration of information in REST is a resource)

* It should use http verbs to express what the request want to accomplish
  * GET and POST are the two we know *so far...*
  * Only GET and POST work in HTML

* Resource information must be part of the url

* By following REST principles, it allows us to design our end points
  * We can follow this convention to help streamline thinking and design

```
Action               HTTP Verb  End Point
--------------------------------------------------------
List all quotes        GET      get '/quotes'
Get a specific quote   GET      get '/quotes/:id'
Display the new form   GET      get '/quotes/new'
Create a new quote     POST     post '/quotes'
Display the form for 
  updating a quote     GET      get '/quotes/:id/update' 
Update the quotes      PUT      put '/quotes/:id' 
Deleting a specific
  quote                DELETE   delete '/quotes/:id'
```

* Nested resources in REST

```
Action               HTTP Verb  End Point
--------------------------------------------------------
Create a new
  comment              POST     post '/quotes/:id/comments'
```

---

### How to use PATCH in Node.js

* We can use middleware to allow these other HTTP verbs, like PATCH

```js
const methodOverride = require('method-override');

app.use(methodOverride('_method'));
app.use((req, res, next) => {
  if (req.query._method) {
    req.method = req.query._method;
    next();
  }
});
```

* In the HTML, we have a POST form, but we change the `action`

```html
<form method="POST" action="/login?_method=PATCH"> <!-- Here -->
  <h3>Login Form</h3>
  <div class="input-group">
    <label for="username">Username</label>
    <input type="username" id="username" name="username" required />
  </div>
  <div class="input-group">
    <label for="password">Password</label>
    <input type="password" id="password" name="password" required />
  </div>
  <br/>
  <button type="submit">Login</button>
</form>
```