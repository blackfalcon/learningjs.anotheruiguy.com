# Context within a function

The simplest way you understand how `this` works is by using the context of an event. Let's say for example you have a series of DOM elements that with a `click` event you want to change the text. But, there are multiple DOM elements and you don't want to put a unique ID on each one.

In the following example we make use of some basic JavaScript features to find all the DOM elements, loop through them, and use the context of `this` to apply the action to only the element that is being interacted with.

To start out, let's build our DOM.

```html
<body>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
</body>
```

Pretty straight forward. Now to get an array of all the DOM elements that we want to be able to click on.

```js
// find all the buttons with this class
var elements = document.getElementsByClassName('foo');
```

Next we can simply loop through the array and apply a click event listener. The magic here is that we are using the keyword of `this` to basically say, "The element that you click on, change `this` text."


```js
// loop through array and add the event of 'click' that will toggle a CSS class
for (var i = 0; i < elements.length; i++) {
  elements[i].addEventListener('click', function() {
    this.innerHTML = "Paragraph changed!";
  });
}
```

What's important to understand here is how we are moving the context of `this` within the scope of this function. To best illustrate this let's wrap this `for()` loop in a function. In this function we can put in a couple of `console.log()` functions and have it log out context of `this`.

What's interesting to understand is that the context of `this` in the function itself is the `window` object. But, within the `for()` loop we have the event listener function, it's in this context when you click on the element, the context of `this` is moved to `[object HTMLParagraphElement]`.

```js
// loop through array and add the event of 'click' that will toggle it's content
function myLoop() {
  console.log(this); // [object Window]

  for (var i = 0; i < elements.length; i++) {
    elements[i].addEventListener('click', function() {
      console.log(this); // [object HTMLParagraphElement]

      this.innerHTML = "Paragraph changed!";
    });
  }
}
```

As you can see in these examples, the concept of `this` is fluid within the scope of functions as to allow for the developer to control the interactions of the user.

To see this in action, check out [this JSBin](https://jsbin.com/voziguc/edit?html,js,output).
