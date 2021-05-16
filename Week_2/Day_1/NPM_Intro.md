# NPM Intro

## `npm init`

* Creates a package.json  with details of the file
  * We can add the name, version, description, main file, and scripts

* We can use `npm install mocha --save` to download the mocha file
  * This creates a `dependencies` in our `package.js`
  * We also have a `node_modules` in our directory

  * You can use `npm install mocha --save-dev`, this saves the framework in a dev environment
    * It will know that mocha is a dev tool

* We can then use `require()` to look for packages in the `node_modules` directory

* `package-lock.json` is generated when we run `npm install` - that comes from the `package.json`

* We **don't** want our git repository to have all the files from **computer-generated** files, such as those in `node_modules`.
  * We can use `.gitignore` to keep these files from entering our repo
  * We can add `node_modules` to the `.gitignore` file to tell git to ignore that file and keep the repo clean.