> {{book.title}} v{{book.version}}

# Callback functions

What is a callback?

> A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.

All this is saying is that when you build a function, there are times when you need to pass in another function to be executed within the scope of the parent function. For example, let's say that you need a generic utility function that prints strings to the console.

```js
function printMe(arg) {
  console.log(arg);
}
```

This by itself is useful, but then imagine that you have another function where you want to take a string and log it to the console as an array.

```js
function arrayMe(arg) {
  const newArray = arg.split('')
  console.log(newArray)
}
```

Again, useful by itself, but now imagine that you have a function that when called you could either output the string as a whole or as an array letter by letter. In this function `callback` is a parameter and then `string` is another. The `callback` argument will replace `callback()` with whatever you pass into the `useMe()` function. And then `string` is simply passed through to the referenced callback function.

```js
function useMe(callback, string) {
  callback(string);
}
```

To use this, we use the `useMe()` function and pass in the callback we want, plus the string.

```js
useMe(printMe, 'this is a callback') // "this is a callback"
useMe(arrayMe, 'this is a callback') // ["t", "h", "i", "s", " ", "i", "s", " ", "a", " ", "c", "a", "l", "l", "b", "a", "c", "k"]
```

## Callbacks in jQuery and click events

The following is a simple example using jQuery to illustrate a common use for callbacks. We have two things happening here.

1. I created the function `alertThing`
1. I then have a jQuery function that passes in `alertThing` as the second argument to the `.on()` function.

So what does this do? Simply put, with the `click` event, the function will run the other defined function. This allows us to take advantage of other functions that are outside the scope of the function we are creating.

```js
function alertThing() {
  console.log('This is a callback!');
}

// event handler -- event -- callback function
$('.click-me').on('click', alertThing);
```

So from this code, we would expect to see a selector in the HTML with the class of `click-me`. Then when we click on this HTML element we would see in the console `This is a callback!`.

What if we didn't want to have this be a fixed value? What if we want this console.log function to allow for the function using it to pass in its own string?

This is totally possible, but the syntax is weird.

First, let's update the function so that we can pass in an argument.

```js
function alertThing(arg) {
  console.log(arg)
};
```

Now the console string can be customized for use. So for the click event, whereas before we were simply passing in `alertThing` as the argument, we need to now define this space as an anonymous function that will use a function that has the argument. In the next example, you will see how you do this.

```js
// event handler -- event -- anonymous function
$('.click-me').on('click', function() {
  // callback -- argument
  alertThing('this is a callback too.')
});
```

### Callbacks and regular JavaScript

Callbacks are not special to jQuery and 1/2 our function is already standard JavaScript. The function `alertThing()` is standard., so that leaves us to re-write the click event and do without the jQuery magic.

In the following example, you will see how I do this. If you want the function to fire immediately, like it does with jQuery, we need to use a wrapper technique called an Immediately-invoked function expression (IIFE) that wraps the function like so ...

```js
({
  ...
})();
```

But first I want to illustrate a basic example where the callback is only referenced and no arguments are passed in.

I have the function of `alertThing()` that will be used as the callback and has the instructions to log a string to the console. Inside the IIFE I am assigning an event to a click handler and assigning the callback to the click handler. You should notice that I am assigning `alertThing` and not `alertThing()`. This is because I want to assign the function itself to the event, not a result.

```js
function alertThing() {
  console.log('This is a callback!')
};

// create the function in the IIFE
(function actionButton() {
  var myButton = document.getElementsByClassName('click-me')[0]; // long hand for finding the .click-me selector

  // Assign the callback to the click event
  myButton.onclick = alertThing;
})();
```

To illustrate how you use a callback inside a function and pass in arguments to that callback function, I simply update the `alertThing()` callback to take an argument. Then inside the `actionButton()` function, I am assigning the the callback to the click event via an anonymous function so that we can assign the argument.

```js
function alertThing(arg) {
  console.log(arg)
};

// create the function in the IIFE
(function actionButton() {
  var myButton = document.getElementsByClassName('click-me')[0]; // long hand for finding the .click-me selector

  // Assign the callback to the click event via an anonymous function so that we can pass in arguments
  myButton.onclick = function() {
    alertThing('this is a callback too.');
  };
})();
```

### More examples make this better ...

Simple clicks with pre-defined strings as a return in the console is one thing, but what about something a little more complex? Let's imagine that we want to make a simple MadLib script that prompts the user for a `noun` or a `verb` and then in the UI the click event is replaced with a Madlib string.

For the HTML, we simply need an element that we can target our click event on.

```html
<div id="clickMe">CLICK ME to make a MadLib!</div>
```

Our first step is to build out the function that will replace the text in the DOM with the new Madlib string. This will be used as a callback a couple of times within another callback.

The function `replaceText()` is pretty generic. As long as you pass in a `string` and a DOM `selector` this function will work anywhere.

```js
// External function to be called back into the executed function
function replaceText(string, selector) {
  selector.innerHTML = string;
}
```

Next, let's build out or word class functions for both a noun and a verb in our Madlib. Both of these functions have a single parameter and will be used as callbacks in the executed function. Within each of these functions, I am using a callback for the `replaceText()` function. Already we are seeing benefits of using callbacks by breaking our code up into smaller reusable functions.

```js
// Noun functon that can be used as a callback
function noun(selector) {
  var madLib = prompt(`Please give me a noun.`);
  var newString = `Poof! You are a ${madLib}`;

  replaceText(newString, selector);
}

// Verb function that can be used as a callback
function verb(selector) {
  var madLib = prompt(`Please give me a verb.`);
  var newString = `Look at me, I like to ${madLib}`;

  replaceText(newString, selector);
}
```

Last we have a few steps to create our `madLibs()` function. The function will consist of two parameters, the `selector` in the DOM we are targeting, and `wordClass` which is basically a callback function we will reference at execution.

The `eventClick` variables value comes from the ID of the `selector` argument.

As for the `onClick` event handler, I am assigning an anonymous callback function and inside that is the `wordClass` argument that will be replaced with the value of the callback passed into the `madLibs()` function on execution.

It should also be noted that that the value of `selector` is assigned to `eventClick` which is passed into the callback which is also passed into the `replaceText()` callback within.

```js
// Executed function that will require click handler and word class arguments
function madLibs(selector, wordClass) {
  var eventClick = document.getElementById(selector);

  eventClick.onclick = function() {
    wordClass(eventClick)
  }
}
```

To execute this function, reference the `madLibs()` function, pass in the selector you are targeting in the DOM and either the `noun()` or `verb()` function as the additional callback.

```js
// Execute function with click handler and word class arguments
madLibs('clickMe', noun);
madLibs('clickMe', verb);
```
