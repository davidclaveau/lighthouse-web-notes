# Command Line Arguments

* Command line arguments are strings of text used to pass additional info to a program whne an application is run through the command line interace (CLI) of an OS.
* Example cases of CLIs:

```
$[runtime] [script_name] [argument-1 argument-2 argument-3 ... argument-n]
```

### Advantages
* We can pass info to an app before it starts, which can be useful to perform large number of configuration settings
* CLI are passe as strings to programs, and can be converted to other data types (very flexible)
* You can pass unlimited number of args via command line
* Used in conjunction with scripts and batch files, which is useful for automated testing

### Disadvantages
* The interface has a steep learning curve, so it's hard for most people to use without a bunch of experience
* Difficult to use without a desktop or laptop, so not typically used on smaller devices like phones or tables

## Using process.argv

* Simple way of retrieving arguments in Node.js
* Global object that you can use without importing any additional libraries
Pass arguments to a Node.js application and these arguments can be accessed within the application via the `process.argv` array
* First element of the `process.argv` array will always be the file system path pointing to the `node` executable. The second is the name of the  JS file being executed. The third element is the first argument that was actually passed by the user.

 Eg.
 ```
 node sum.js 10 25
 ``` 
 Would produce the following:

 ```
 [
  '/home/vagrant/.nvm/versions/node/v12.22.1/bin/node',
  '/vagrant/w1/d1-focal/sum.js',
  '10',
  '25'
]
```
We get the path to the node executable `'/home/vagrant/.nvm/versions/node/v12.22.1/bin/node'` and then we have the file path to our `sum.js` file (`'/vagrant/w1/d1-focal/sum.js'`); finally, we have the elements that we passed into `sum.js`, which were **25** and **10**.

#### _Don't forget!_

  * Arguments that are passed in are strings, so you'll need to convert any "numbers" to their correct data type.