# Web Servers

## Review on TCP
* We have to establish a connection between client and server
* This connection has to be constant
* Either party can send information at any time
  * Think of it like a pipe, where either party can yell a message that echoes to the other end of the pipe

## HTTP
* Built on top of the constant TCP connection, but that's not how the web works
* HTTP requires a specific request
  * A **request -> response cycle**
* We need to know the address of the computer we're communicating with
  * Address of the server: the *host/domain* (`localhost`)
  * One host can have multiple applications
    * We need to know the particular **port** that this web app is hosted on
  * In summary: we need the **domain** and the **port**

* Everything on the internet has to be assigned a **internet protocol** address

## Communicating via HTTP

* We have to put together a `request` to the server
  * Can ask you do something, or ask you to give me something
  * We can indicate these using HTTP verbs
    * `GET` or `POST`

* Also need to specify what we want to do it to
  * We specify this with the path/url
    * For example, the `/users`, `/maps`, `/photos`

* We will use the combination of verbs and paths to communicate with the server

---

## Using Node for HTTP requests

* In the NPM notes, we can download the `http` package

* `http.createServer` can help us with creating a server

* If we find `listener` or `handler` will always indicate a callback
  * This will always be asynchronous, this will always happen in the future

```js
const http = require('http');

// Returns an instance of a server
// So let's capture it in a variable, const server
const server = http.createServer((request, response) => {
  response.write('Hello world');
  response.end(); // We have to terminate each response
});
```

* We're always sent a request and we need to post a response

* We have to make sure we assign a specific `port`

```js
const http = require('http');

const server = http.createServer((request, response) => {
  response.write('Hello world');
  response.end();
});

server.listen(34765); // <---
```

* Every time a request comes in, the same response comes through with `'hello world'`

* Information about the `request` is in the `request` object.

* We need the `url` and the `method` of the request, which is inside the `request` object

```js
const server = http.createServer((request, response) => {
  console.log(request.url); // <---
  console.log(request.method); // <---

  response.write('Hello world');
  response.end();
});

// GET
// /home
// GET
// /favicon.ico
```

* This is enough to make conditional statments about what's incoming

```js
const req = `${request.method} ${request.url}`;
```

* Because they come in pairs, they are stated together

```js
const server = http.createServer((request, response) => {

const req = `${request.method} ${request.url}`;

if (req === 'GET /users') {
  response.write('you have requested all the users');
  response.end();
} else if (req === 'GET /home') {
  response.write('you are at the home page');
  response.end()
} else {
  response.write('page does not exist');
  response.end()
}

  // response.write('Hello world');
  // response.end();
});
```

* We won't actually use `http` from Node, but we would rather use Express.

## HTTP Status Codes

* Status codes tell the browser how the request went
* 304, 200, 404, 500
* https://http.cat/ is a great library of http statuses
* We can specify the conde in our responses

## Middleware

* Sits between the `request` and the `response`
  * Runs before the code - the `route-handlers`
* Fantastic for parsing
  * Turn info from one format to another
  * We use middleware to:
    * Grab the cookies and put it into req.cookies
    * Grab the body and put into req.body
    * USe it for encryption

## Templating / Server-side Rendering

* We're going to take a template from the server, feed it a bunch of data, the server is going to interpolate the data, and then send it back to the browser.

* We need:
  1. A template
  2. Data


