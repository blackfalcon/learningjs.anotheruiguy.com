# Functions

The thing I want to talk about in there is that in JavaScript, everything is really a function, but there use and output is what separates them.

In JavaScript, functions are first-class objects, this is because they can have properties and methods just like any other object. The role of a function is to be a mini-program that returns a value either directly or via another function within its closure.

There are two common types of functions, the Declarative and the Expression. The following example illustrates these common constructs.

```js
// Function Declaration
function theFunctionDeclaration(prop1, prop2) {
  const value = prop1 + prop2;

  return value
}

console.log(theFunctionDeclaration(10, 20)); // 30

// or

let theFunctionExpression = function(prop1, prop2) {
  const value = prop1 + prop2;

  return value
}

console.log(theFunctionExpression(10, 20)); // 30
```

## ES6 and Functions

With the implementation of ES6, there are some new things that we can do with functions.

### Fat Arrow

There is a lot to digest on the concepts of the Fat Arrow function `=>`, and I will deep dive on that later. But know for now, one of the great things we can do with the Fat Arrow is rethink how we implement the Function Expression. In the following example I am updating the previous example of a Function Expression with that of the Fat Arrow syntax.

```js
let theFunctionExpression = (prop1, prop2) => {
  const value = prop1 + prop2;

  return value
}

console.log(theFunctionExpression(10, 20)); // 30
```

### Spread syntax

The Spread Syntax is a pretty cool concept that allows you to create a function that will allow for `n` number of arguments that you do not need to pre-define.

Let's say for example you need to have a function that takes in an list of names and, prints out the number of items in that list and prints out the contents of that list. Using traditional JavaScript you would have to parse through the list in the array, get it's `.length` and then loop through all the items in the list individually and print them out to the console.

