> {{book.title}} v{{book.version}}

# Intro to functions

In JavaScript, so many things are a function. There are standard functions that simply run a process and it outputs a result. Then there are things like methods, which are functions on an object. A callback is where another function is passed in as arguments to another function. A closure is a function within a function that returns a value within its scope. A constructor function is a function used to template the creation of objects. And last, there is a curry function which is a way of breaking up complex bundles of code within a single function.

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

### Why use one over the other?

When using JavaScript you will be faced with the question of, "_Why use a declaration versus an expression?_" The simple answer to this is hoisting.

In this first example, we see that the order of things is not happening in the order they are written. This is because function declarations are hoisted. The second function `foo()` is hoisted above the return and thus over-rides the first `foo()` function.

```js
function foo(){
  function foo() {
    return `1st function`;
  }

  return foo();

  function foo() {
    return `2nd function`;
  }
}

console.log(foo()); // "2nd function"
```

Let's look at another example, this time with a function expression. Here the first function is returned, but why? With hoisting, the variable statement is hoisted, but the value is not. The value of the variable is loaded into memory in the same order that the code is written.

```js
function foo(){
  var foo = function() {
    return `1st function`;
  }

  return foo();

  var foo = function() {
    return `2nd function`;
  }
}
console.log(foo()); // "1st function"
```

If this example is confusing, the following is an example of how this code is loaded into memory. Here you can see that the second declaration for `foo()` is past the return statement of `foo()`.

```js
function foo(){
  // variable statement
  var foo;

  // foo variable declaration
  foo = function() {
    return `1st function`;
  }

  return foo();

  // foo variable declaration
  foo = function() {
    return `2nd function`;
  }
}
console.log(foo()); // "1st function"
```

Many JavaScript developers having a coding preference of one over the other, but in the end their use is interchangeable based on the order of code being written and depending on the level of abstractions in your code there may be a preference for function expressions as you have some level of control as to when the function is loaded into memory.

## Functions and parameters

Functions are very versatile in how they can be used. In many examples, you will see functions that take a series of parameters, and these typically translate directly to a variable within the function as illustrated in the following example.

```js
function sum(num, num2, num3) {
  return num + num2 + num3
}

const foo = sum(1, 2, 3) // 6
```

### The spread and rest parameter syntax

In the previous example there is a clear 1:1 parsing of the input parameters and the arguments within the function, and in most cases, this will not be an issue. In the cases where the input is more variable, this syntax can be very limiting. For example, the following code illustrates that by passing in additional parameters, this has no effect on the output, good or bad.

```js
function sum(num, num2, num3) {
  return num + num2 + num3
}

const foo = sum(1, 2, 3, 4, 5, 6) // the answer is still 6
```

It is here where the `spread` and `rest` parameter syntax, `...`, can help solve this problem. While the `...` syntax is the same, their applications are clearly different.

> The rest parameter syntax allows us to represent an indefinite number of arguments as an array.

> Spread syntax allows an iterable such as an array expression or string to be expanded ...

What does this mean? In the case of the `rest` parameter syntax, an unrestricted number of argument inputs become a single array parameter.

Looking at the previous example, the `rest` parameter syntax can be used to replace the individual parameters. As stated, this will turn the input into an array and as a result the inner workings of the `sum()` function needs to be updated to loop through the values of the input array.

```js
function sum(...args) {
  let answer = 0
  for (let i=0; i<args.length; i++) {
    answer += args[i]
  }

  return answer
}
```

With this update, the new `sum()` function can accept an unlimited number of parameters and output an answer.

```js
const args = sum(10, 13, 4, 7, 12) // 46
```

Given that the `sum()` function has been updated with the `rest` syntax and the input parameters are now an array, one could assume that an array could be pass into the `sum()` function. This assumption would be incorrect as this would be considered a single parameter and this function would return `0` plus the array.

```js
const array = sum([10, 13, 4, 7, 12]) // "010,13,4,7,12"
```

As a 'dirty hack', developers started using the `apply()` method on the Function as this allows for arguments to be passed in as an array. This is a 'dirty hack' is because of the use of `null` in the function in place of the `thisArg` parameter in the method.

```js
// Syntax
// func.apply(thisArg, [argsArray])

const applyArray = sum.apply(null, [10, 13, 4, 7, 12]) // 46
```

The `spread` parameter syntax, on the other hand, can avoid the 'dirty hack' altogether. The `spread` parameter syntax differs from the `rest` parameter syntax because of its direct application of an array as a parameter when executing a function. This syntax is illustrated in the following example.

```js
const spreadArray = sum(...[10, 13, 4, 7, 12]) // 46
```

A combination of these parameter syntax types and this function, inputs are virtually unlimited.

```js
const spreadArray = sum(...[10, 13, 4, 7, 12], ...[1, 2, 3], 100, 50, 50, ...[1, 2, 3]) // 258
```

Don't forget, the `spread` parameter syntax allows `an iterable such as an array expression or string` to be used. That means, using the `spread` syntax one can pass in a string and it will be iterated over like an array.

```js
const spreadArray = sum(...[1, 2, 3], ...' = sum total') // "6 = sum total"
```


## Functions and objects

As your code gets more complex, building functions where users need to fill in a large number of parameters becomes increasingly complex to follow. In cases like this, it may be simpler to pass in a whole object of data. For example, a function that needs to parse the properties of a single object passed into it.

In this example, once the object of `seattle` is passed into the function `theWeather()`, all its properties are now bound to the `city` variable. Knowing the API of the object we can easily construct a string asking for `object.property` and the function will return its corresponding value.

Nested property patters like `object.property.property` are easily followed as well.

```js
const seattle = {
  sky : 'cloudy',
  winds: {
    speed: 'calm',
    direction: 'east'
  },
  temp: '43ºF',
  percep: 'light rain'
}

function theWeather(city) {
  console.log(`Today's weather is ${city.sky} with a temp of ${city.temp} and ${city.percep}. Winds are ${city.winds.speed} out of the ${city.winds.direction}.`)
}

theWeather(seattle);
// "Today's weather is cloudy skies with a temp of 43ºF and light rain. Winds are calm out of the east."
```

With this function, we can pass in any city data. For example, what about Boston?

```js
const boston = {
  sky : 'clear',
  winds: {
    speed: 'calm',
    direction: 'south west'
  },
  temp: '41ºF',
  percep: 'no precipitation'
}

theWeather(boston);
// "Today's weather is clear with a temp of 41ºF and no precipitation. Winds are calm out of the south-west."
```

## Functions and an array of objects

As your data gets even more complex, functions are again up to the task. In this example, we have an array of data and we need to iterate over the objects in the array, evaluate the value of a property and make a decision based on return.

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

To parse through this data we need to write a function that will loop through all the objects within the array. We can do this using a `for ... of()` loop and bind the data of the object to `this`.

```js
// Our function using this
function parseEvents() {
  for (let event of this) {
    event.attend ? console.log(`${event.event} at ${event.location}`) : ''
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
    event.attend ? console.log(`${event.event} at ${event.location}`) : ''
  }
};
```

Here is the function passing in the object `myEVents` into the function.

```js
// Using function by passing in arg
parseEventsWithParam(myEvents);
```

## Functions and block scope

ES6 introduced many new concepts on scope and one is the ability to contain the scope of a function not only within itself but also within a block statement.

Take the following for example. The variable `foo` is in the outer most scope or the global scope. Following that we have two versions of the `funct()` functions. But one is contained within a block `{}` statement.

When logging the output of `funct()` we will get the return of `"non-block scope"`.

```js
var foo = '';

function funct() {
  foo = 'non-block scope'
  return foo;
}

{
  function funct() {
    foo = 'block scope'
    return foo;
  }
}

console.log(funct()) // "non-block scope"
```
