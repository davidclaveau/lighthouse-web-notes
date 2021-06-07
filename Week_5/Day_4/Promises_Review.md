# Promises

### `Promises` overview

```js
const query = "SELECT id, name FROM users";

const promise = pool.query(query);

console.log(promise);
```

* This will show the result as `Promise { <pending> }`  in the console, as it is running asynchronously

* We need to use `.then` to get something out of the response

```js
const query = "SELECT id, name FROM users";

const promise = pool.query(query);
console.log(promise);

promise.then(res => {
  console.log(res);
});
```

* This will get us the result we want from our promise

* And we can use `res.rows` to get into the object that's returned in the Promise

---

### `.then()`

* We can chain our `.then`s and pass the values along to other `.then`

* You can chain promises 

```js
promise = pool.query("SELECT id, name FROM users;")
  .then(res => {
    console.log(res.rows);
    return pool.query("SELECT id, name FROM widgets;")
  })
  .then(res => {
    return 5;
  });
```

* This will return the promise we call, and pass it into the next `.then`

---

### `.catch()`

```js
promise = pool.query("SELECT id, name FROM users;")
  .then(res => {
    console.log(res.rows);
    return pool.query("SELECT id, name FROM widgets;")
  })
  .then(res => {
    return 5;
  })
  .catch(err => {
    console.log(err);
  });
```

* Catch will catch anything above it, such as errors in the promise or errors in the syntax

---

### `.finally()`

* `.finally` is the last thing that **will always run** with a promise
  * Takes no parameters

* Particularly useful for database functions

```js
promise = pool.query("SELECT id, name FROM users;")
  .then(res => {
    console.log(res.rows);
    return pool.query("SELECT id, name FROM widgets;")
  })
  .then(res => {
    return 5;
  })
  .catch(err => {
    console.log(err);
  })
  .finally(() => {
    pool.end() // close our pool off
  });
```

---

### HTTP `Promise`

* `axios` is a very popular module, used in react
  * Used to make calls to web services

```js
const axios = require('axios');

const url = "https://api.kanye.res";

const promise = axios.get(url);

promise.then(res => {
  console.log(res.data);
});
```

* We get a fun quote from Kanye that changes every time it's called

```js
const axios = require('axios');

const url = "https://api.kanye.res";

const promise = axios.get(url);

promise
  .then(res => {
  console.log(res.data);
  })
  .catch(err => {
    console.log(err)
  })
```

---

### `Promise.all`

* This allows us to execute several promises all at once
  * This means we have to handle the `res` in our `.then` as an array

```js
const axios = require('axios');

const promise1 = axios.get("https://api.kanye.res");
const promise2 = axios.get("https://api.kanye.res");
const promise3 = axios.get("https://api.kanye.res");

Promise.all(promise1, promise2, promise3)
  .then(res => {
    console.log(res[0].data);
    console.log(res[1].data);
    console.log(res[2].data);
  })
  .catch(err => {
    console.log(err)
  })
```

* If one promise fails though, they will always fail

* What we can do is make all these promises `.catch` individually

```js
const promise1 = axios.get("https://api.kanye.res").catch(e => e);
const promise2 = axios.get("https://api.kanye.res").catch(e => e);
const promise3 = axios.get("https://api.kanye.res").catch(e => e);
```

* This is processsed in the `.then()` following the `Promise.all`

```js
const axios = require('axios');

const getKanye = function () {
  axios.get("https://api.kanye.res")
    .then(res => {
        return res.data;
    })
};

getKanye(); // returns undefined
```

* This doesn't return the `res.data`, as that's only passed to the promise. We need to `return` the promise.

* Our function is now **then-able**

```js
const axios = require('axios');

const getKanye = function () {
  axios.get("https://api.kanye.res")
    .then(res => {
        return res.data;
    })
};

getKanye()
  .then(res => {
    console.log(res);
  })
```