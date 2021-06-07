# Express

* We want to use environment keys to hide the important stuff on Github

```js
const config = {
  host: process.env.DB_HOST,
  user: process.env.DB_HOST,
  name: process.env.DB_HOST,
  password: process.env.DB_HOST,
}
```

* We use `dotenv` on Express
  * This creates a file called `.env` that we **gitignore**
  * We add the `DB_HOST` information into this file

---

* Set up a `db.js` that connects to the database

* Then we can call the `db` file and use it in our `product-queries.js`
  * We can use a promise to get the query and pass back the values

```js
const getProducts = () => {
  return db.query(`SELECT * FROM products;`)
    .then((response) => {
      return response.rows
    });
};
```

* We can create our `product-router.js`
  * This creates the `GETS` and the `POSTS` that we can export
  * This will call our `product-queries.js`

`product-router`
```js
const express = require('express');
const router = express.Router();
const productQueries = require('../db/product-queries');

// GET /api/products
router.get('/', (req, res) => {
  productQueries.getProducts(); // <--- get our Promise query
    .then(products) => {
      res.json(products);
    }
})

// GET /api/products/:id
router.get('/', (req,res) => {
  productQueries.getProductById(req.params.id)
    .then((product) => {
      res.json(product)
    })
})

module.exports = router;
```

* In our `server.js` file, we can call the routers
  * `/api` means you're only getting data, like JSON or XML

* We set our path and call the router file

`server.js`
```js
/// import routers
const productRouter = require('.routes/product-router');

app.use('/api/products', productRouter)
```


---

* The skeleton file for the project combines HTTP requests and SQL queries into one callback

```js
const express = require('express');
const router = express.Router();

const postRouterFn = (dbConnection) => {

  // GET /api/posts
  router.get('/', (req,res) => {
    dbConnection.query('SELECT * FROM posts;`)
      .then((response) => {
        res.json(response.rows)
      })
  })

  // GET /api/posts/:id

};

module.exports = postRouterFn;
```

`server.js`
```js
/// import routers
const postRouter = require('.routes/post-router');

app.use('/api/posts', postsRouter(db));
```