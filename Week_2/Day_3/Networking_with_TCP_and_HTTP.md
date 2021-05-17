# Networking with TCP and HTTP

### Intro Discussion

* The **web** are the *pages you see* when you're at a device and you're online. The the **internet** is the *network of connected computers* that the web works on
  * *Internet* was invented by DARPA in the 60s
  * *Web* was invented at CERN

* URL - Uniform Resource Locator

* We go to https://www.cbc.ca
  * We see it's a GET request
  * We see there are **headers**
    * They negotiate a bunch of things, like what's requested and accepted
      * Images
      * Text
  * There are cookies within the page with data
  * See the user-agent, so the webpage knows who is looking at it and how to respond based on the user
  * In the **response** we see the HTML of the webpage, so *text* wrapped in *tags* to provide semantics of the page.

* Servers use **ports** to accept TCP requests

---

* We can use Node to create a server
* Node comes with a package to create servers using `require('net')`

```js
const net = require('net')
const PORT = 9898;

const server = net.createServer();

// An event listener
// On connection to the server, do _____ (callback)
server.on('connection', () => {
  console.log('Client is connected!')
});

// An event listener
// Takes a port and a callback function
server.listen(PORT, () => {
  console.log(`Server is listening on port ${PORT}`);
});
```

* We can use the `net` package to set up different event listeners
* This is called setting up an event listener, so it exists in the *asynchronous event loop*

* We need to create another file for the **client** to access the server and activate those event listeners

```js
const net = require('net');
const PORT = 9898;

// This object needs the port and host
// (just localhost in the example)
const configObj = {
  port: PORT,
  host: 'localhost'
};

// To use the `createConnection`, we need to use `configObj`
const client = net.createConnection(configObj);

// When the 'connect' event happens, that's when
// the callback is called and will run
client.on('connect', () => {
  console.log('client is connected to the server.')
});
```

* The server will listen for connections and will run its callback function when the client connects
  * We see the "Client is connected!" when the client connects to the server

* A lot of information can be added to the server when the client is connected:

```js
const net = require('net')
const PORT = 9898;

const server = net.createServer();

// We can add a `client` parameter
server.on('connection', (client) => {
  console.log('Client is connected!')

  // And we can write to the client
  client.write('Welcome to my awesome server!')

});

server.listen(PORT, () => {
  console.log(`Server is listening on port ${PORT}`);
});
```

And we can write what the client receives:

```js
const net = require('net');
const PORT = 9898;

const configObj = {
  port: PORT,
  host: 'localhost'
};

const client = net.createConnection(configObj);

client.on('connect', () => {
  console.log('client is connected to the server.')
});

// This event can take a parameter for the data received
client.on('data', (message) = {
  console.log('server sent:', message)
});
```

* However, the client will receive *raw data* from the server when it receives the `client.write()`

* We need to encode the data in `UTF-8`

* We need to add the following:

```js
server.on('connection', (client) => {
  console.log('Client is connected!')

  client.setEncoding('utf8'); // <--
  client.write('Welcome to my awesome server!')

});
```

* And we need to set it in the client file as well:

```js
const client = net.createConnection(configObj);

client.setEncoding('utf8'); // <--

client.on('connect', () => {
  console.log('client is connected to the server.')
});
```

* If, on the client side, you want to send messages out

* We can use `process.stdin` to receive information from the client and send it out

```js
process.stdin.on('data', (message) => {
  console.log(`sending ${message} to server`);
  client.write(message);
});
```

* FYI, it doesn't matter if we change the order of our asynchronous functions in the file.
  * When passed to the events loop, the order doesn't matter

* Then we can create an event listener on the server

```js
server.on('connection', (client) => {
  console.log('Client is connected!')

  client.setEncoding('utf8');
  client.write('Welcome to my awesome server!')

  // We add this:
  client.on('data', (message) => {
    console.log(`received from client: ${message}`);
  });
});
```

---

## TCP

* Each request you make to a web server has a *verb* attached to it
  * **GET** - *read* data from the server
  * **POST** - *send* data and *create* an object on the server
  * **PUT/PATCH** - *send* data and *update* an object on the  server
  * **DELETE** - *delete* data on the server

* Only two of these can be used on a web server (GET and POST).

* You also get a status code, such as 200 and 404

* When a client makes a connection on the server, we would like to keep an array of those clients *globally* on the server

```js
const connectedClients = [];
```

* And within the `server.on('connection', () => {})` we add:

```js
const connectedClients = [];

// Checks the currentClient and broadcasts the message to the other client
const broadcast = (currentClient, message) => {
  for (let connectedClient of connectedClients) {
    if (connectedClient !== currentClient) {
      connectedClient.write(`${message}`);
    }
  }
};

server.on('connection', (client) => {
  console.log('Client is connected!')

  client.setEncoding('utf8');
  client.write('Welcome to my awesome server!');

  client.on('data', (message) => {
    console.log(`received from client: ${message}`);
    broadcast(client, message); // Broadcasting which client and what msg
  });
  
  // Add the current client to the list of connected clients
  connectedClients.push(client);
});
```

* Lastly, we can add an `end` to the client file

```js
client.on('end', () => {
  console.log('client disconnectedfrom server.')
})
```