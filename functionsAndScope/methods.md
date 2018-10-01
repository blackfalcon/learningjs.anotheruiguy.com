> {{book.title}} v{{book.version}}

# Object methods

Methods are in essence functions, but it's their context that defines their difference. Methods are functions that are to be performed specifically on the properties of an object. For example, there are a number of pre-defined methods in JavaScript that help us find the properties of an object.

Keep in mind that regular functions defined in the global scope are bound to the `window` object and thus are now simply methods of the `window` object. In JavaScript, context is king!

Great examples are the commonly used `sting` and `array` methods. In the following example, we have a string that is being passed into a function. Inside the function, we are creating two variables using methods on the string object.

```js
// define the myString object
const myString = "This is a pretty awesome string, just saying.";

function aboutMyString(string) {
  // using .split() method to convert the string into an array
  let stringArray = string.split(' ');

  // using the .length() method on the string object to get a count of the substrings in the array
  let arrLength = stringArray.length;

  console.log(arrLength);

}

aboutMyString(myString); // 8
```

Using the Functions example above, I can make a change putting the `console.log()` statement inside the function and then I can call the function as a method on the `window` object.

In the example below I am calling the function `theFunctionDeclaration()` as a method on the `window` object.

```js
function theFunctionDeclaration(prop1, prop2) {
  const value = prop1 + prop2;

  console.log(value);
}

window.theFunctionDeclaration(10, 20); // 30
```

Function Expressions, on the other hand, behave slightly different, because the function expression itself is stored inside a variable. In the following example you see how we need to assign the function expression to a variable and now I can call the variable as a method on the `window` object.

```js
let theFunctionExpression = (prop1, prop2) => {
  const value = prop1 + prop2;

  console.log(value);
}

var foo = theFunctionExpression;

window.foo(10, 20); // 30
```

Trying to get `theFunctionExpression()` directly, you would get an error.

```js
TypeError: window.theFunctionExpression is not a function
```

## Building custom methods

What do we know about methods so far? Basically, there are methods baked into JavaScipt that we can take advantage of right away to access properties of our objects. Great. But, what if we need to do something custom? Well, given the construct of JavaScript itself, we know that the value of a key inside an object can be a function. So, all a custom method is, is a function assigned to a key value within the scope of an object.

There are a couple of ways to do this. One way requires a constructor, and a way to bind a method to that new prototype.

```js
function Person(first, last) {
  this.firstName = first;
  this.lastName = last;
}
```

Ok, now that we have an object constructor, let's write the new method that will be bound to the new object when it is created from this constructor.

```js
Person.prototype.sayMyName = function() {
  console.log(`${this.firstName} ${this.lastName}`);
}
```

In order to get this to actually do anything, we need to use the constructor to create the object of `dad` where we will append the name data.

```js
const dad = new Person('Thomas', 'Newcastle');
```

Boom, we have a new object for `dad`. Now that we have this setup, we can call the object and use the method to say my name.

```js
dad.sayMyName(); // "Thomas Newcastle"
```

This example illustrates the long-hand way of doing this work. What's interesting is if you `console.log()` the `dad` object. It comes back looking like this.

```js
[object Object] {
  firstName: "Thomas",
  lastName: "Newcastle",
  sayMyName: function () {
    window.runnerWindow.proxyConsole.log(`${this.firstName} ${this.lastName}`);
  }
}
```

The `sayMyName` method is simply inside the object. So this clearly illustrates how you can build the method directly inside the object constructor, like in the following example.

```js
function Person(first, last) {
  this.firstName = first;
  this.lastName = last;
  this.sayMyName = function() {
    console.log(`${this.firstName} ${this.lastName}`);
  }
}
```

Setting up the constructor to have the method directly inside does exactly the same thing as appending the method to the prototype.

```js
const dad = {
  firstName: 'Thomas',
  lastName: 'Newcastle',
  sayMyName: function() {
    console.log(`${this.firstName} ${this.lastName}`)
  }
}

dad.sayMyName(); // "Thomas Newcastle"
```
