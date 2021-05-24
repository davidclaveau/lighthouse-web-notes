# Response Design and SASS

* Elements and design of the page that *responds* to the device that is viewing the page

* Mobile devices account for %50 of web traffic worldwide

* It's something to consider when you hear "mobile-first design"
  * Create the layout for mobile devices first (more restrictions)
  * Extend the layout for larger screens
  * Somewhat forced to keep certain design principles, only expanding when the appropriate real estate on the page is available

## Responsive Design

* Multitude of different screen sizes exist across phones, tablets, desktops, tvs and weables

* Responsive design means that your web app can adapt to any screen size and provide a good user-expererience.

* With responsive web design, the server always sends the same HTML code all devices, and CSS is used to alter the rendering of the page on the device

### Responsive Design Tools

* **Relative units** `(%,` `em`, `rem`) help the page grow and shrink and keep the relative aspect ratios.
  * Most important for in-between sizes

* **Media queries** allow you to set specific styles within set boundaries
  * These are often called *break points*

* **Responsive Images** is related to the system where the client that requests the image file receives an image appropriate to the page's render size.

### View Port

* The **viewport** tag instructs the browser how to adjust the page to the width of each device.

* When the meta viewport element is absent, mobile browsers will display web pages with default desktop settings. This results in a seemingly zoomed out, unresponsive design.

### Units

#### Absolute Units

* `cm`, `mm`, `in`, `pt`, `px`

#### Relative Units

* `em` is relative to the size of the **parent**
* `rem` is relative to the size of the **root element**
* `vw` is relative to 1% of the **viewport width**
* `vh` is relative to 1% of the **viewport height**
* `%` is relative to the percentage of the **parent size** (width, heigh, font-size)

### Media Queries

* Media queries will allow you to use different CSS style rules according to various screen sizes

* Consists of a `media-type and (media-features)`s
  * Media type: screen, print, handheld, tv, projector, all

```css
@media only screen and (max-width: 600px) {...}
@media only screen and (min-width: 600px) {...}
@media only screen and (max-width: 600px) and (min-width: 400px) {...}
```

* You can have `screen` and `print` media queries
  * `print` means the designer is thinking of people wanting to print the page off (remove useless elements, deliver content)
  * For example, removing `nav` and `a` tags from the document

```css
@media print {
  nav, .ad {
    display: none
  }
  a {
    text-decoration: none;
    color: inherit;
  }
}
```

* A **breakpoint** is when we want the new style to begin

* We can also link to a specific css file for media queries

```html
<link rel="stylesheet" href="small.css" media="(max-width:600px)">
```

* This stylesheet is only for when the page is < 600px

* Not necessarily better, as this is a separate file to download; also, sometimes preferrred to have media queries available in one file.

* We can also use the `not` operator for media queries to specify everything else

```css
@media not screen {...} 
```

* We can also comma-separate our media queries to apply to more than one queries

```css
@media not screen, screen and (max-width: 400px) {...}
```

### Responsive Images

* Have a range of media queries for different images
* HTML won't get angry about a weird `<picture>` tag

```html
<picture>
  <source srcset="./images/cats_200.jpg" media=" (max-width: 200px)">
  <source srcset="./images/cats_400.jpg" media=" (max-width: 400px)">
  <source srcset="./images/cats_600.jpg" media=" (max-width: 600px)">
  <source srcset="./images/cats_800.jpg" media=" (max-width: 200px)">
  <source srcset="./images/cats_1000.jpg" media=" (max-width: 200px)">

  <img class="cat-img" src="./images/cats_1200.jpg">
</picture>
```

### Type Scale

* A collection of carefully picked font sizes that we use to represent different text elements in order to establish a balanced and harmonious composition in our products.

* By consistently using the same font sizes from our own type scale, we can automatically instill uniformity in our design and avoid a product that looks all over the place in terms of hierarchy and prioritizes


### Responsive Web Design Patterns

#### Mostly Fluid

* Consists primarily of a fluid grid. Larger screens, usually the same size and simply adjusts the margins on wider screens.
* On small screens, the fluid grid causes the main content to reflow, while columns are stacked vertically.
* Usually only need one breakpoint between small screens and large screens

#### Column Drop

* For full-width multi-column layouts, column drop simply stacks the columns vertically as the window width becomes too narrow for the content.
* Eventually, this results in all of the columns being stacked vertically. This pattern is dependent on the content and changes for each design.

#### Layout Shifter

* The layout shifter pattern is the most responsive pattern, with multiple breakpoints across several screen widths
* Requires modification to elements more than to content layout, and therefore can be more complex to maintain.

#### Tiny Tweaks

* Small changes to the layout, such as adjusting font size and resizing images in very mminor ways
* Primarily only good for single column layouts and text-heavy articles

#### Off Canvas

* Rather than stacking content vertically, the off canvas pattern places less frequently used content - such as nav or app menus - off screen. Only showing when the screen is large enough

--- 

## SASS

* Is a CSS compiler

* We can use `@import` to bring in variables from other files
  * You need to have the other .css named with `_` so it's like `_variables.css`
  * Then we can import the variables

```scss
/* In _variables.css */
$font-color: red;
$border-color: blue;

/* In other file */
@import 
```

* We can nest our CSS elements with SASS like so:

```scss
/* Normal css */
.container p {
  color: magenta;
text-decoration: underline;
}

.container div {
  border: 1px solid black;
}

/* Nesting with SASS */
.container {
  p {
    color: magenta;
    text-decoration: underline;
  }
  div {
    border: 1px solid black;
  }
}
```

* We can use `&` like it's `this` for scss. So we can use it for pseudo-class

```scss
.container {
  p {
    color: magenta;
    text-decoration: underline;
  }
  &:hover {
    border: 1px solid black;
  }
}
```