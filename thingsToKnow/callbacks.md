# Learning callbacks

The term 'callback' always confused me. I didn't really understand what was happening. But the simple answer is this ...

> A callback is simply when a function is using another function as an argument.

In the following example we have two things happening here.

1. I created the function `alertThing`
1. I then have a jQuery function that passes in `alertThing` as the second argument to the `.on()` function.

So what does this do? Simply put, with the `click` event, the function will run the other defined function. This allows us to take advantage of other functions that are outside the scope of the function we are creating. In this case, anytime

```js
function alertThing() {
  console.log('This is a callback!');
}

$('.click-me').on('click', alertThing);
```

So from this code we would expect to see a selector in the HTML with the class of `click-me`. Then when we click on this HTML element we would see in the console `This is a callback!`. But what if we didn't want to have this be a fixed value? What if we want this console.log function to allow for the function using it to pass in it's own string?

This is totally possible, but the syntax is weird.

First, let's update the function so that we can pass in an argument.

```js
function alertThing(arg) {
  console.log(arg)
};
```

There, now the console string can be customized on use. So for the click event, whereas before we were simply passing in `alertThing` as the argument, we need to now define this space as an anonymous function that will use a function that has the argument. Don't you just lOVE JavaScript? In the next example, you will see how you do this.

```js
$('.click-me').on('click', function() {
  alertThing('this is a callback too.')
});
```

And yes, jQuery can use the ES6 fat arrow syntax

```js
$('.click-me').on('click', () => {
  alertThing('this is a callback too.')
});
```


### No jQuery man!

So there you have it. But this is jQuery man ... what about doing this in regular JavaScript? The good news is that callbacks are not special to jQuery and 1/2 our function is already standard JavaScript. The `alertThing()` function. So that leaves us to re-write the click event and do without all the jQuery magic.

In the following example you will see how I do this. If you want the function to fire immediately like it does with jQuery, we need to a wrapper technique that wraps the function like so ...

```js
({
  ...
})();
```

I know, weird. But ok. Then we need to do a little more work to find the selector and the click event is a little different too. In the end you will have something that looks like this.

```js
// create the function in the self-executing wrapper
(function actionButton() {
  var myButton = document.getElementsByClassName('click-me')[0]; // long hand for finding the .click-me selector

  // looks a lot like the jQuery callback
  myButton.onclick = function() {
    alertThing('this is a callback too.');
  };
})();
```

And yes ... we can use the `() =>` syntax here too.

```js
(function actionButton() {
  const myButton = document.getElementsByClassName('click-me')[0];

  myButton.onclick = () => {
    alertThing('this is a callback too.');
  };
})();
```

### More examples make this better ...

Ok, let's say that we want to make a simple MadLib script that prompts the user for a `noun` and then in the UI the click event is replaced with a MadLib string.

For the HTML, we simply need an element that we can target our click event on.

```html
<div id="clickMe">Let's make a MadLib!></div>
```

Next, lets build out the function that will replace the text in the DOM with the new MadLib string. This is what we will use as the callback in the main function. The function is going to be a generic module we can use anywhere where you can pass in the new string and the ID of the DOM element we are targeting.

```js
function replaceText(string, selector) {
  selector.innerHTML = string;
}
```

Next let's build out the function we need that will target the DOM element, apply the click event and then callback the `replaceText()` function.

```js
function madLibs() {
  var eventClick = document.getElementById('clickMe');

  eventClick.onclick = () => {
    var madLib = prompt(`Please give me a noun.`);
    var newString = `Poof! You are a ${madLib}`;

    replaceText(newString, eventClick);
  }
}

madLibs();
```
