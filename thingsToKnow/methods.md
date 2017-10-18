# Methods

Methods are in essence functions, but it's their context that defines their difference. Methods are functions that are to be performed on the properties of an object, not used directly in the global scope as you would a Function Declaration or Expression. For example, there are a number of pre-defined methods in JavaScript that help up find the properties of an object.

A great examples are the commonly used `sting` and `array` methods. In the following example we have a string that is being passed into a function. Inside the function we are creating two variables using methods on the string object.

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

aboutMyString(myString);
```

Keep in mind that functions defined in the global scope are bound to the `window` object and thus are now methods of the `window` object. Remember, in JavaScript, context is king!

Using the Functions example above, I can make a change putting the `console.log()` statement inside the function and then I can call the function as a method on the `window` object. Notice in the example below I am calling the function `theFunctionDeclaration()` as a method on the `window` object.

```js
function theFunctionDeclaration(prop1, prop2) {
  const value = prop1 + prop2;

  console.log(value);
}

window.theFunctionDeclaration(10, 20);
```

Function Expressions, on the other hand, behave slightly different, because the function expression itself is stored inside a variable. In the following example you see how we need to assign the value from the function expression to a variable and now I can call the variable as a method on the `window` object.

```js
let theFunctionExpression = (prop1, prop2) => {
  const value = prop1 + prop2;

  console.log(value);
}

var foo = theFunctionExpression(10, 20);

window.foo;
```

## Building custom methods

See [JSBin](https://jsbin.com/nirapiy/edit?js,console) for examples shown here.

What do we know about methods so far? Basically there are methods baked into JavaScipt that we can take advantage of right away to access properties of our objects. Great. But, what if we need to do something custom? Well, given the construct of JavaScript itself, we know that the value of a key inside an object can be a function. So, all a custom method is, is a function assigned to a key value within the scope of a function.

To do this, there are a couple of ways. One way requires a constructor, and a way to bind a method to that new prototype. Yeah, it is about as messy as it sounds. Ok, first thing first. Let's create a constructor that we will build our object from.

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

Ok, cooking with gas now. In order to get this to actually do anything, we need to use the constructor to create the object of `dad` where we will append the name data.

```js
const dad = new Person('Dale', 'Sande');
```

Boom, we have a new object for `dad`. Now that we have this set up, we can call the object and use the method to say my name.

```js
dad.sayMyName(); // "Dale Sande"
```

This example illustrates the long-hand way of doing this work. What's interesting is if you `console.log()` the `dad` object. It comes back looking like this.

```js
[object Object] {
  firstName: "Dale",
  lastName: "Sande",
  sayMyName: function () {
    window.runnerWindow.proxyConsole.log(`${this.firstName} ${this.lastName}`);
  }
}
```

WAT?! The `sayMyName` method is simply interjected inside the object? So this clearly illustrates how you can build the method directly inside the object constructor, like in the following example.

```js
function Person(first, last) {
  this.firstName = first;
  this.lastName = last;
  this.sayMyName = function() {
    console.log(`${this.firstName} ${this.lastName}`);
  }
}
```

Setting up the constructor to have the method directly inside does exactly the same thing as appending the method to the prototype. And what if we wanted to dismiss the constructor all together?

```js
const dad = {
  firstName: 'Dale',
  lastName: 'Sande',
  sayMyName: function() {
    console.log(`${this.firstName} ${this.lastName}`)
  }
}

dad.sayMyName(); // "Dale Sande"
```

I, myself, see a lot of value creating constructors when possible. It helps to set a constant pattern of object creation and leaves you with a lot of opportunities down the road having access to the object's prototype.

Read more on [constructors](/thingsToKnow/constructors.html)
