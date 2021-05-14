# Command Line Tips and Tricks

* System for interacting with the files on your computer, following this template:

```
$> command[options][filename-path][filename-path]...
```

* Even "commands" are files, so searching `which ls` will show the file path (`/bin/ls`)

---

### Techniques

* Special Directory Names
  * `cd ~` goes to the root
  * `cd -` goes back to last directory, regardless of where you are
* Fileame Completion
  * ls ~/ligh[TAB]
    * Tab will complete the command line
    * `ls ~/ligh[TAB][TAB]` will show all relevant files that start with `ligh`
    * `ls ../[TAB][TAB]` will list all files one directory up
* History search magic trick
  * We can type the letter of a previous command, and hit the `up arrow` to find the commands you entered with that letter
  * Can use the `!` operator and type the number in the history for that command to be used
  * Last word of the previous command: `!$`
    * `mv ~!$` (move last word of previous command to root `~`)
* Line Editing Shortcuts
  * CMD-A moves to **start** to the command line
    * Great way to save what you've already typed. Use CMD-A to go back to the start and make a fake command like that doesn't exist; save it in history and come back to it.
  * CMD-E to the **end** of the command line
  * CMD-K deletes from cursor to end of the command line
  * CMD-D delete character under the cursor

### Avoid typing `ls -> cd -> ls`

* The easiest way to do this is to type `cd ./` and then double TAB to see all files, then finish your command.

### Pipes and Redirects

* "Pipe" one command's output into another command's input.

* For example, we can do something like

```
$> cat solution/*.js | grep "res"
```

* `cat` will output from the file
* `grep` will search the file for instances where we have "res"
* So we search through solution for a js file (`*.js`) and look through it.