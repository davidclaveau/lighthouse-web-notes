# Intro to CSS

## HTML 5 Semantic Tags

* There are roughly 100 **semantic elements** available in HTML 5
  * `<article>`, `<footer>`, `<nav>`, `<main>`, etc.
  * Hints for a particular context of the page
  * Serve purposes for the developer as well as bots/crawlers
    * Google crawling a webpage looking for specific data searched by the user
    * Also important for screen readers to find information; helps render information for the user

---

## Box Model

* Almost every HTML element can be represented by a box (or rectangle)
* We can see on a webspage how tags and content create boxes and are nested within other boxes.
* There's the style of the `height` and `width` of the box
  * We can adjust the `padding`, the `margin`, and the `border`
* There's `content-box` and `border-box`
  * With `border-box`, changing the padding or border of a box will not change the height and width
    * It shrink the content area within the padding/border
    * A lot less math involved
    * **Margin** still affects the box
    * Think of it as the `border` being the edge of the content
    * `border-box` is much more desired in builds, as it allows for **pixel-perfect** designs
    * [Content-box vs border-box](https://guyroutledge.github.io/box-model/)

---

## Element Layout

* We can set **inline styling** using the `style` attribute in a tag
  * Attributes are like metadata, in a way

```html
<img style="float: left; margin: 5px" width="100px">
```

* `<span>` is the best example of an **inline** element
  * Injected into the line and doesn't create a new line
* `<div>` is a great example of a **block** element
  * Takes up the full width by default

* `float` is used to place everything in one area of the container
  * How to place everything in the `container` class inline, as opposed to be stacked on top of each other

```html
<main class="container"">
  <img width="100px">
  <img width="100px">
  <img width="100px">
</main>
```
* Using `float` on the nested elements will shrink the container to contain just the elements nested within it

```html
<main class="container"">
  <img style="float: left; margin: 5px" width="100px">
  <img style="float: left; margin: 5px" width="100px">
  <img style="float: left; margin: 5px" width="100px">
</main>
```

## Flexbox

* [Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox)
* Flex is based on the two types of styles
  * Styles that apply to the **parent** elements
  * Styles that apply to **children** elements

* `Display` is the property that, historically, you set the value for to specify `block` or `inline` elements
  * You set up a container to have the `display` property of `flex`
    * This container's children will be able to use the `flex` property

```css
.container {
  display: flex;
}
```

* `Flex-direction` is used to establish the main-axis of the items in the container

```css
.container {
  flex-direction: row;
}
```

* `Flex-wrap` specifies what will happen when children elements go so far as to be wider than the max width of the container

```css
.container {
  flex-wrap: wrap;
}
```

* `Flex-flow` is a shorthand for the `flex-direction` and `flex-wrap` properties, which together define the flex container’s main and cross axes. 

```css
  .container {
    flex-flow: row nowrap;
  }
```

* `Justify-content` spaces things out just to one specified style

```css
  .container {
    justify-content: flex-start;
  }
```

* `Align-items` is the best example of the **main axis** compared to the **cross axis**
  * The direction of the main axis was set by the previous style `flex-direction`
    * `Row` or `row-reverse` is a **horizontal axis**
    * `Column` or `column-reverse` is a **vertical axis**
  * We use `align-items` to set the content against the cross axis.
    * Think of it as the `justify-content` version for the cross axis (perpendicular to the main-axis)

```css
  .container {
    align-items: stretch;
  }
```

* `Order` lets us assign numbers so the largest number child container is at the end of the container compared to other children containers

* `Flex-grow` is used to grow a child container within the parent container. The larger the number will be a larger container. Sum of all numbers will indicate the percentage of the containers.
  * E.g. 1-2-1 means that the 2 will be twice the size. 

* We can't use `flexbox` for getting items to wrap around other content
  * We need to rely on `float` 

## `em`

* An `em` is a unit of measurement. However, unlike a `pixel` which has a fixed absolute size, the size of an `em` is relative to its parent's **font** size.

* For example, if the font-size for a <div> is 16px, then 1`em` of space in that <div> is 16px. 1.5`em` in that <div> would be 24px, 2`em` would be 32px, etc. By using `em` to specify things like margin, border, and padding, the spacing of those things will automatically change if a change is made to the font size.

## CSS Refresher

* We can use the Chrome Dev Tools to see the `Elements` and the `Styles` that are applied to an element
  We can go to `Computed` to see *all* the styling that's applied (and not applied) to an element

## Specificity Calculations

* There's a calculation on specificity of styling
  * The system pays attention to which style is the most specific to an element.
* Specificity = (1 * number of elements) + (10 * number of classes) + (100 * number of IDs) + (1000 * inline styles)
  * This is often represent with dash-separated numbers `0-0-0-0`
  * The highest number applied to an element from this calculation would be showed in the browser.
  * [CSS Specificity Examples](https://cssspecificity.com/)
  * [More on Specificity](https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/)