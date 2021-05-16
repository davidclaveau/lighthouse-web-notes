# Chrome Dev Tools

## CSS

Shortcuts  
  1. Command Menu
  * Cmd + Shift + P
  2. Run Dock to Bottom, Right, or Separate Window
  * Cmd + Shift + P and selecting "Undock"

* `Elements` > `Computed` pane to see a declaration and its cascade

* Navigating the DOM Tree
  * Cmd + Shift + C to inspect a specific element
    * Hovering over the element shows some essential CSS
  * Cmd + F to search by strings, CSS selectors, or even `XPath` queries
  * You can `Console` > `inspect(element)`
    * inspect(document.querySelector('input')) to highlight an element in the DOM tree

* Command Menu > Animations tab
  * Able to see the animations as they appear on the page

* Command Menu > Rendering tab
  * We can use Paint Flashing to see which animations are not rendering correctly and efficiently

* Prototyping Basics
  * Using the element in `Styles` to instantly make changes

* Autocomplete by value
  * Inspect an element and make bold, we can just type 'bold' and Chrome Dev Tools will complete the property for us automatically.

* Pseudo-classes
  * Go to `Styles`
  * Click `.hov` to see the hover tag applied to the page

* Classes
  * Go to `Styles`
  * Click `.cls` will allow us to apply other classes to an element

* Contrast Ratio
  * Inspect an element
  * Find the element's `color` declaration
  Click the small box next to `color` value
  * That expands the *Contrast ratio*

* Copy Element Styles
  * Right-click > Copy > Copy Styles
  * Go back to `Styles` and paste the CSS in the `Elements` section

* Screenshots of page, area, node

## JS

* Console Shortcuts
  * Cmd + Option + J

* Logging Basics
  * Sometimes inefficient, but very reliable:
  * `console.log({pets})`
    * Prints out the name of the variable, followed by the value
  * `console.table(pets)`
    * Give it an array of objects, prints a tidy table for you
  * `console.trace('how is this?)`
    * Investigating a new codebase and not sure how you got to a particular place, use this to see trace history.

* Debugging with `Sources` panel
  * Better suited for tricky bugs
  * We enter a `debugger` statement into our code
    * We can step-over our code one line at a time
    * We can enter a function as needed
    * We can also keep track of values in our code
      * In the `Scope` pane we can see what's defined in the local scope
      * We can also open up the `Console` pane, so it evaluates where we are in the step process
        * Ask `what's value of <variable>?` and console the value
  * Right-click on the line in `Sources` and select `Logpoint` to add a `console.log` for that line to the console

* More breakpoints
  * DOM mutation breakpoints
    * Right-click an element and select `Break on` > `attribute modifications`
      * Will pause when that element is changed
  * In `Sources`, we can select `event.listener` we can select options like `Mouse` and enable `click` event to set another breakpoint and pause on line of code where this occurs.
    * If in another `js` file, we can right-click and `blackbox` the script to create a breakpoint in our file.

* Analyzing load performance
  * Audits panel will run audits and tell you the exact issues with the page
    * You can see opportunities for performance improvements