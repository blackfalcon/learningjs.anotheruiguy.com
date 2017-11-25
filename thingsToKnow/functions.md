# Functions

In JavaScript, everything is really a function. There are standard functions that simply run a process and it outputs a result. Then there are things like methods, which are functions on an object. Callbacks, which are functions that are passed in as arguments to another function. Closures, which are functions within functions that return a value within its scope. Constructors that are functions used to template the creation of objects. And last, there is currying, which is a way of breaking up complex bundles of code within a single function.

Functions are first-class objects, this is because they can have properties and methods just like any other object. The role of a function is to be a mini-program that returns a value either directly or via another function within its closure.

There are two common styles of writing functions, the Declarative and Expressive. The following example illustrates these common constructs.

```js
// Declarative
function theFunctionDeclaration(prop1, prop2) {
  const value = prop1 + prop2;

  return value
}

console.log(theFunctionDeclaration(10, 20)); // 30

// or Expressive

let theFunctionExpression = function(prop1, prop2) {
  const value = prop1 + prop2;

  return value
}

console.log(theFunctionExpression(10, 20)); // 30
```

## Functions and parameters

Functions are very versatile in how they can be used. In many examples, you will see functions that take a series of parameters, and these typically translate directly to a variable within the function as illustrated in the following example.

```js
function foo(arg, arg2, arg3) {
  let bar = arg + arg2 + ar3;
  return bar;
}
```

As your code gets more complex, building functions where users need to fill in a large number of parameters becomes increasingly complex to follow. In cases like this, it may be simpler to pass in a whole object of data. For example, let's imagine that you have calendar data and you need to write a function that will parse this data and return a subset for use.

```js
// Our data
const myEvents = [
  {
    event: 'Lisa\'s Birthday party',
    start: 'Wed Nov 22 2017 16:00:00 GMT-0800 (PST)',
    location: 'Pizza Palace on Gilman',
    attend: true
  },{
    event: 'Game night',
    start: 'Sat Nov 25 2017 10:00:00 GMT-0800 (PST)',
    location: '1844 East Olympia Way',
    attend: false
  },{
    event: 'Turkey Leftovers',
    start: 'Fri Nov 24 2017 12:00:00 GMT-0800 (PST)',
    location: 'W22645 Woodland Ln.',
    attend: true
  }
];
```

To parse through this data we simply need to write a function that will loop through all the objects within the array. We can do this using a `for ... of()` loop and bind the data of the object to `this`.

```js
// Our function using this
function parseEvents() {
  for (let event of this) {
    if (event.attend) {
      console.log(`${event.event} at ${event.location}`)
    }
  }
};
```

Since we are using `this` to bind to the object, when this function is called either the `call()` or `apply()` method could need to be used since there are no additional arguments being passed into the function.

```js
// Use function with call() method
parseEvents.call(myEvents);
```

This exact same function could be written without using the `.call()` or `.apply()` methods by simply requiring that the object is passed into the function when used.

```js
// Our function requiring the passing in of the object
function parseEventsWithParam(obj) {
  for (let event of obj) {
    if (event.attend) {
      console.log(`${event.event} at ${event.location}`)
    }
  }
};
```

Here is the function passing in the object `myEVents` into the function.

```js
// Using function by passing in arg
parseEventsWithParam(myEvents);
```

<!-- ## ES6 and Functions

With the implementation of ES6, there are some new things that we can do with functions.

### Fat Arrow

There is a lot to digest on the concepts of the Fat Arrow function `=>`, and I will deep dive on that later. But know, for now, one of the great things we can do with the Fat Arrow is rethinking how we implement the Function Expression. In the following example, I am updating the previous example of a Function Expression with that of the Fat Arrow syntax.

```js
let theFunctionExpression = (prop1, prop2) => {
  const value = prop1 + prop2;

  return value
}

console.log(theFunctionExpression(10, 20)); // 30
```

### Spread syntax

The Spread Syntax is a pretty cool concept that allows you to create a function that will allow for `n` number of arguments that you do not need to pre-define.

Let's say for example you need to have a function that takes in a list of names and, prints out the number of items in that list and prints out the contents of that list. Using traditional JavaScript you would have to parse through the list in the array, get it's `.length` and then loop through all the items in the list individually and print them out to the console.

-- >
