> {{book.title}} v{{book.version}}

# Higher-order functions

The concept of higher-order functions is probably something that you are already familiar with. The basic definition of a higher-order function is;

1. a function that takes a function as an argument, or
1. a function that returns a function

Functions in JavaScript are interesting things. You may hear them referenced as first-class functions. What this means is, functions are essentially objects as they have properties and methods. E.g. there is a `function.prototype` chain and methods like `.apply()`, `.call()` and `.bind()`. While a function does have a prototype chain, you cannot create additional methods on a function.

A quick look at function properties. Here we see that I have created the function `groot()` and I am referencing the `name` property in the `console.log()` statement. This may feel repetitive in this simple, but the uses are pretty vast.

```js
function groot() {
  console.log('I am Groot')
}

console.log(groot.name); // "groot"
```

A quick look at a functions method, `bind()`. Here I have a simple object literal and then the `groot()` function. Alone, running the `groot()` function would not work as `this` has the context of the `window` object. So, using the `.bind()` prototype method, I can use the data of `me` to bind `this` to the `groot()` function.

```js
const me = {
  name: 'Groot'
}

function groot() {
  console.log(`I am ${this.name}`)
}

const iAmGroot = groot.bind(me)
iAmGroot() // "I am Groot"
```

Knowing that a function has properties and methods like an object, it now makes sense that we can pass functions around as arguments into other functions, as well, return a function from a function. The more you can be comfortable with that statement, the better you will accept JavaScript.

## Common higher-order functions in Web Programming

If you have programmed anything to work on the web, you are already using common higher-order functions. For example, let's imagine that we have two DOM elements and by clicking on each one, you want to get their IDs. For starters, we need to assign each DOM element to a variable that we can reference in our code.

```js
const eventTrigger = document.getElementById('click-me');
const otherEventTrigger = document.getElementById('click-there');
```

The next step is to create a reusable function that we will use later in our click events. Alone, this function is worthless. No clicks will respond to this function and `this` has no context at all. So, we need to bind this function to another function, or what is referred to as a click handler.

```js
function consoleClick() {
  console.log(`You clicked on element ID ${this.id}`);
}
```

For this to work, we need to use the variable object we created (the `EventTarget`), append the `addEventListener()` method which requires an object implementing the EventListener interface, or ... **a JavaScript function**.

```js
eventTrigger.addEventListener('click', consoleClick);
otherEventTrigger.addEventListener('click', consoleClick);
```

And there you have it. A higher-order function that requires another function to be passed into it which will perform the necessary action needed once the `click` event has been triggered.

## Higher-order function that returns a function

For the second description of a higher-order function, a requirement is to return a function. Again, you may already be doing this as this is a common pattern for a closure. In this following example of a higher-order closure function, we see `character()` accepts a single argument. This argument is used to create a string and following is the private variable of `punc`. The closure function of `characterStr()` can reach outside to the parent scope and retrieve the two values.

What makes this a higher-order function is that the `return` of `character()` is the `characterStr` function itself. If you were to `console.log(character())` would see the `characterStr()` as its return in the console.

```js
function character(string) {
  const concatString = `I am ${string}`;
  const punc = '!';

  function characterStr() {
    console.log(concatString + punc);
  }

  return characterStr;
}
```

To make use of this function and its returned function, we can bind the function and its return to a new variable. Once that is done, we can call the new variable as a function and it will return the `console.log` statement within.

```js
const groot = character('Groot');
groot(); // "I am Groot!"
```
