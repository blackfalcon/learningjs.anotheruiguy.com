> {{book.title}} v{{book.version}}

# Context within a function

One of the simplest concepts of `this` that we can start with is how `this` is contrived from an event. And the event, in this context, is any interaction a human user would have with your application. Be it either a click, tap or mouse gesture.

For example, you have a series of DOM elements that with a `click` event you want to change the text as it appears in the browser. But, in this example, there are multiple DOM elements. One way to encapsulate each event is to tag each DOM element with a unique ID and write a JavaScript function that will look for that ID and apply the action based on the event. That doesn't sound very maintainable, scalable, reusable, or DRY?

In an ideal world, each DOM element would be tagged with a single, reusable reference, and a single function to do the work. This way, any future developer who would want to use this action needs only to place the appropriate tag on the DOM element.

To illustrate this concept I will make use of basic JavaScript features to find all the DOM elements. Once found I will loop through them, and use the context of `this` to apply the action to only the element that is being interacted with.

To begin, let's build out the DOM we will use. In this example, I have four strings that when clicked the JavaScript function will remove the current string and replace it with the one I designated in the function. Notice that there is an element that does not have the class of `changeStr`. Clicking on that element will do nothing.

```html
<body>
  <p class="changeStr">This is a generic string of words.</p>
  <p>Clicking on me will do nothing.</p>
  <p class="changeStr">Clicking here will change me and only me.</p>
  <p class="changeStr">Clicking here will change me and only me.</p>
</body>
```

In this next example, I am performing a method on the document object that will return an array of all the DOM elements that contain the class of `changeStr`.

```js
// find all the buttons with this class
const elements = document.getElementsByClassName('changeStr');
```

Next, we can simply loop through the array and apply a `click` event listener. The magic here is that we are using the keyword of `this` to basically say, "The element that you click on, change `this` text."


```js
// loop through array and add the event of 'click' that will toggle a CSS class
for (var i = 0; i < elements.length; i++) {
  elements[i].addEventListener('click', function() {
    this.innerHTML = "Paragraph changed!";
  });
}
```

What's important to understand here is how we are moving the context of `this` within the scope of this function. To best illustrate this let's wrap this `for()` loop in a function. In this function, we can put in a couple of `console.log()` functions and have it log out the context of `this`.

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
