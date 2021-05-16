# jQuery

* jQuery provides many conveniences when working with the DOM, events, and much more

* For example, looking for height and width of an element in the browser (with Internet Explorer having issues with window.innerWidth)

```js
const getViewPortWidth = function() {
  let IEDocument = document.documentElement;
  if(window.innerWidth) {
    return window.innerWidth;
  } else if(IEDocument.clientWidth) {
    return IEDocument.clientWidth;
  } else if(IEDocument.getElementsByTagName('body')[0]) {
    return IEDocument.getElementsByTagName('body')[0].clientWidth;
  }
}
const viewportWidth = getViewPortWidth()
```

* But this can be summzarized in jQuery like so:

```js
$(window).width()
```

Regular JS can be a little longer and convoluted to write out:

```js
const element = document.getElementById("foo");
element.addEventListener("click", function() {
  alert("Clicked!");
});
```

* Where as the `event` handles in jQuery are a bit more condensed and easier to read when doing the same thing:

```js
$("#foo").on( "click", function() {
  console.log("Foo element clicked");
});
```

* Or:

```js
$("#foo").click(function() {
  console.log("Foo element clicked");
});
```