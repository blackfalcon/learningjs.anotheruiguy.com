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

Need to really flush this out here. Refer to the `objectWithMethods` example to illustrate this here.
