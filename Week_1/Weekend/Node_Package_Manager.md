# Node Package Manager

## Packages

* Node's package manager **npm** allows us to install and use open-source JS packages.

* Packages (*package libraries*) are self-contained code libraries that we can install and use in our projects

* The command for **npm** is rather straightforward:

```
npm install <NAME>
```

* This downloads the package into the current directory/project that we're in.

## package.json

* Virtuall all Node.js projects have a file called `package.json`, which looks similar to this:

```js
{
  "name": "project-name",
  "version": "1.0.0",
  "description": "Short project summary",
  "main": "index.js",
  "scripts": {
    "myscript": "ENV=development node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "MIT",
  "dependencies": {
    "express": "^4.13.4"
  }
}
```

* Which has some basic attributes such as the project's name, description, and author

* The `scripts` portion allows us to run commands using an alias, for instance

```
npm run myscript
```

* The `dependencies` sections of `package.json` lists the packages that need to be installed for the project to run properly.
  * In the example above, we see `express` and `4.13.3` as the version
  * After running `npm install`, you well find dependencies in `./node_modules`
  * `node_modules` should NOT be committed to your project's git repo because pacakge.json will recreate the node_module. It would be redundant and would cause giant git commits every time a library is updated.

* Running `npm init` will prompt you with question to create your own package.json (which can be edited  later).

* (After downloading the `chalk` package) we see that there's a `package-lock.json` file
  * The `package-lock.json` file lists all the details about our **project's dependencies**  it should be checkde into git, along with `package.json`
  * More info about package.json [here](https://docs.npmjs.com/cli/v7/configuring-npm/package-lock-json)


* `package-lock.json` is gitignored in a repo, whereas package.json is not ignored.

* Semantic versioning is a common practice where you use a [MAJOR.MINOR.PATCH] number system to provide your code/library to the user. 