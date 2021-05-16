# Event Handling and User Input

* A rather common requirement of user-facing software is to handle user input
  * User clicking a button on a page
  * Submitting a form
  * Or drag and dropping something

* These are called **events** and can happen at anytime while an app is running - which means it needs to be handled **asynchronously**

* Callbacks are a major way that JS handles dealing with code that needs to run later

* For example:

```js
// on any input from stdin (standard input), output a "." to stdout
process.stdin.on('data', (key) => {
  process.stdout.write('.');
});

console.log('after callback');
```

* This bit of code looks for input, and writes out a ' . ' each time a key is pressed.

* We can add the following snippet within the `process.stdin.on` function to add a cancel/exit button.

```js
// \u0003 maps to Control + C input
if (key === '\u0003') {
  process.exit();
}
```

## Node and `readline`

* We can use Node's `readline` to read one line at a time. It can use any input stream (we're currently only interested in `stdin`)

```js
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

rl.question('What do you think of Node.js? ', (answer) => {
  // TODO: Log the answer in a database
  console.log(`Thank you for your valuable feedback: ${answer}`);

  rl.close();
});
```

* This asks the user a `question`, and allows information that is inputted to be acted on.