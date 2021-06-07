# SQL From Our Apps

## Accessing the Database from the Command Line

* We will be using the `pg` package to access our PostgreSQL database using javascript

```js
const pg = require('pg');
```

* You then create a `Client` class

```js
const Client = pg.Client;
```

* And we have our `configObj`

```js
const configObj = {
  user: 'david.claveau',
  host: 'localhost',
  database: 'foo'
}
```

* Then the `Client` class connects to our `configObj`

```js
const client = new Client(configObj);
```

* We can use a promise to catch handling

```js
client.connect()
.then(() => console.log("db connected"))
.catch(() => console.log("db connection error");)
```

* And we can use `process.argv` to take in arguments from the command line:

```js
const verb = process.argv[2];
```

* We can then use a `switch` statement to support the browse verb

  * Have the `default` be to specify a verb we understand, and use `client.end()` to end the connection
  
  * `client.query()` is a promise, so we can add `.then()` onto it 

  * Our verbs will be BREAD - browse, read, edit, add, delete

```js
const verb = process.argv[2];

switch(verb) {
  case 'browse':
    client.query("SELECT * FROM objectives ORDER BY id;")
    .then((response) => {
      console.log(response)
      .client.end();
    });
    break;

  default:
    console.log("Please enter a verb that I understand: browse, ...");
    client.end()
    break;
}
```

* When we call `response` in the `.then()` function, we see that one of the responses coming from the table is in an `array` of *rows* with an `object` of values

```
row: [
  {
    id: 123
    day_id: 13
    type: performance
    ...
  }
]
```

* So we can call `response.rows`


```js
const verb = process.argv[2];

switch(verb) {
  case 'browse':
    client.query("SELECT * FROM objectives ORDER BY id;")
    .then((response) => {
      console.log(response.rows) // <--- Look at specific row
      .client.end();
    });
    break;

  default:
    console.log("Please enter a verb that I understand: browse, ...");
    client.end()
    break;
}
```

* We can now add `read`, `edit`, `add`, and `delete`
  * We can use a new variable `const id = process.argv[3]` to get an associated id we're looking for
  * This allows us to use string interpolation to find `id` from the command line into the queries.

```js
const verb = process.argv[2];
const id = process.argv[3]; // <--- add another argument
const newQuestion = process.argv[4] // <--- for our `edit`

switch(verb) {
  case 'browse':
    client.query("SELECT * FROM objectives ORDER BY id;")
    .then((response) => {
      console.log(response)
      .client.end();
    });
    break;

  case 'read':
    client.query(`SELECT * FROM objectives WHERE id = ${id};`)
    .then((response) => {
      console.log(response)
      .client.end();
    });
    break;

  case 'edit':
    client.query(`UPDATE objectives SET question = '${newQuestion}' WHERE id = ${id};`)
    .then((response) => {
      console.log(response)
      .client.end();
    });
    break;

  // TODO
  // case 'add':
  //   client.query(`SELECT * FROM objectives WHERE id = ${id};`)
  //   .then((response) => {
  //     console.log(response)
  //     .client.end();
  //   });
  //   break;

  case 'delete':
    client.query(`DELETE FROM objectives WHERE id = ${id};`)
    .then((response) => {
      console.log(response)
      .client.end();
    });
    break;

  default:
    console.log("Please enter a verb that I understand: browse, ...");
    client.end()
    break;
}
```

--- 

## How to avoid SQL Injection Attacks

* How do we handle unwanted text from entering our database?

* Instead of a normal query, like `"edit 1 change question etc."` we add the following:

```
"1; DELETE FROM objectives WHERE id = 1;" 
```

* This will delete our row using our query - a query on another query.

* What we do is take our *untrusted input* out from the query
* We add a token in its place
* There's a second argument in the query that has variables substituted in for the tokens
* When the query with the attack tries to access `id` in the database, it sees there is a string attached to the variable
    * `id` *only* takes `INTEGER` so the system runs an error and stops the attack

* In the case of `edit`
  * `$1` and `$2` are our tokens
  * We pass in an array with the user input as those variables

```js
case 'edit':
    client.query(`UPDATE objectives SET question = $1 WHERE id = $2;`, [newQuestion, id])
    .then((response) => {
      console.log(response)
      .client.end();
    });
    break;
```

---

* With our app, we combine what we've learned with our database knowledge

```js
const express = require('express')
const app = express();
app.set('view engine', 'ejs')

const pg = require('pg');
const Client = pg.Client;

const configObj = {
  user: 'david.claveau',
  host: 'localhost',
  database: 'foo'
}

const client = new Client(configObj);

client.connect()
.then(() => console.log("db connected"))
.catch(() => console.log("db connection error");)
```

---

## Combining Database Queries with Express and our Apps

* We should modularize our database and files with databases as well

* So we create a `database` or `db` directory
  * We store our helper functions in this directory

* We should also have our queries in a separate directory, so a `queries.js` file
  * We bring in our `connection.js` file to access the object and the `Client` object we created.
    * This has our `Client` and `configObj`, etc.
  * This will have our helper functions

`queries.js`
```js
const client = require('./connection')

const getAllObjectives = (cb) => {
  client
    .query(`SELECT id,type,question,answer,sort,day_id ORDER BY id FROM objectives ORDER BY id;`)
    .then((response) => {
      cb(response.rows);
    })
    .catch((err) => {
      console.log("db getAllObjectives error:", err);
    });
};

module.exports = {
  getAllObjectives
}
```

* We go back to `index.js`, we add that file:
  * `dbFns` will grab our helper function from `queries.js`
  * This means we need a view for our `index.ejs` to be rendered.

`index.js`
```js
const express = require('express')
const app = express();
app.set('view engine', 'ejs')

const dbFns = require('./db/queries');

app.get('/', (req, res) => {
  dbFns.getAllObjectives((rows) => {
    console.log('rows', rows);
    const templateVars = {
      objectives: rows
    };
    res.render('index', templateVars)
  });
});

const port = process.env.PORT || 8080;
app.listen(port, () => {
  console.log(`app is listening on port ${PORT}`);
});
```

* We can add a new route for `get` and `post` to add a new question/answer to our database
  * Don't forget that we need our `bodyParser` to read the input from the `form`


`index.js`
```js
const express = require('express')
const app = express();
app.set('view engine', 'ejs')

const bodyParser = require('body-parser')

// Middleware
app.use(bodyParser)

const dbFns = require('./db/queries');


app.get('/', (req, res) => {
  dbFns.getAllObjectives((rows) => {
    console.log('rows', rows);
    const templateVars = {
      objectives: rows
    };
    res.render('index', templateVars)
  });
});

app.get('/new', (req, res) => {
  res.render('new');
});

app.post('/new', (req, res) => {
  const newObj = {
    question: req.body.question,
    answer: req.body.answer
  }
  dbFns.insertObjective(newObj);
  res.render('new');
});

const port = process.env.PORT || 8080;
app.listen(port, () => {
  console.log(`app is listening on port ${PORT}`);
});
```

* We go back to `queries.js` and create that helper function for accessing the database
  * This time we are `INSERT`ing information into the database
  * We're only going to be adding `question` and `answer`, so we can remove `id`, `type`, etc.
  * We also need to add tokens into the query to prevent SQL injection

`queries.js`
```js
const client = require('./connection')

const getAllObjectives = (cb) => {
  client
    .query(`SELECT * FROM objectives ORDER BY id;`)
    .then((response) => {
      cb(response.rows);
    })
    .catch((err) => {
      console.log("db getAllObjectives error:", err);
    });
};

const insertObjective = (newObjective) => {
  client.query(
    "INSERT INTO objectives(question,answer) VALUES ($1,$2);",
    [newObjective.question,newObjective.answer]
  )
    .then((response) => {
      console.log(response.rows);
    })
};

module.exports = {
  getAllObjectives,
  insertObjective
}
```

* And we create a view for `new` and add a `form` to add new information to our database

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
<form action="/new" method="POST">
  <label for="question">Question</label>
  <input id="question" type="text" name="question" />

  <label for="answer">Answer</label>
  <input id="answer" type="text" name="answer" />

  <input type="submit" />
  
</body>
</html>
```

---

## Hiding Secrets from your Git Repo

* We can use a package `dotenv` and add as early as possible

```js
require('dotenv').config()
```

* Then we use `env.example .env` to create a `.env` file
  * Inside the file we have

```
HOST='localhost'
USER='david.claveau'
DB_NAME='foo'
...
```

* And then in the fie with our host, user, and db info, we call the `process.env.NAME` to replace it

```js
const configObj = {
  user: process.env.USER,
  host: process.env.HOST,
  database: process.env.DB_NAME
}
```



---

### How the callback in getAllObjectives works

`queries.js`
```js
const client = require('./connection')

const getAllObjectives = (cb) => {
  client
    .query(`SELECT id,type,question,answer,sort,day_id ORDER BY id FROM objectives ORDER BY id;`)
    .then((response) => {
      cb(response.rows);
    })
    .catch((err) => {
      console.log("db getAllObjectives error:", err);
    });
};

module.exports = {
  getAllObjectives
}
```

* We go back to `index.js`, we add that file:
  * `dbFns` will grab our helper function from `queries.js`
  * This means we need a view for our `index.ejs` to be rendered.

`index.js`
```js
const express = require('express')
const app = express();
app.set('view engine', 'ejs')

const dbFns = require('./db/queries');

app.get('/', (req, res) => {
  dbFns.getAllObjectives((rows) => {
    console.log('rows', rows);
    const templateVars = {
      objectives: rows
    };
    res.render('index', templateVars)
  });
});

const port = process.env.PORT || 8080;
app.listen(port, () => {
  console.log(`app is listening on port ${PORT}`);
});
```

* Gets us this... or something:

```js 
const getAllObjectives = (cb) => {
  client
    .query(`SELECT id,type,question,answer,sort,day_id ORDER BY id FROM objectives ORDER BY id;`)
    .then((response) => {
      (rows) => {
        console.log('rows', rows);
        const templateVars = {
          objectives: rows
        };
        res.render('index', templateVars)
      }
    })
    .catch((err) => {
      console.log("db getAllObjectives error:", err);
    });
};

(rows) => {
    console.log('rows', rows);
    const templateVars = {
      objectives: rows
    };
```
