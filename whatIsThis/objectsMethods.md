> {{book.title}} v{{book.version}}

# The context of 'this' in an object and method

In JavaScript objects have properties and methods. Methods are functions that when applied will mutate or expose their data. Methods can either be written directly within the object itself or applied via its prototype. It is the context of the method that is important to understand as this will have implications for the `this` property.

To begin to illustrate this concept, let's create a small object literal that describes `myVehicle`.

```js
const myVehicle = {
  make: 'Toyota',
  model: 'Tacoma',
  color: 'black',
  year: 2017,
  options: 'AWD and tow package'
}
```

For our method, we will create it directly within the context of the object itself as illustrated in the following updated example.

```js
const myVehicle = {
  make: 'Toyota',
  model: 'Tacoma',
  color: 'black',
  year: 2017,
  options: 'AWD and tow package',
  printObj: function() {
    console.log(`I drive a ${this.color} ${this.year} ${this.make} ${this.model} with ${this.options}`)
  }
}
```

What's important to notice is that the `printObj` method is bound to the `myVehicle` object directly by context. This is a very basic example of Object Oriented Programming in JavaScript.

To execute this function, we call the object and its method which will return the string we are creating.

```js
myVehicle.printObj(); // "I drive a black 2017 Toyota Tacoma with AWD and tow package"
```

To make this a little more interesting, consider that we have not only `myVehicle`, but `secondVehicle` as well. Now I'd hate to have to create this method twice so instead we can create a new object using `myVehicle` as the prototype like so. Remember, `Object.assign()` will assign all the current properties and methods to a new object, but we need to use `Object.create()` in order to generate a new object with the over-written properties of that new object.

```js
const secondVehicle = Object.assign(Object.create(myVehicle), {
  make: 'Toyota',
  model: 'Rav4',
  color: 'Silver',
  year: 2003,
  options: '5 speed manual transmission'
})
```

At this point we can run the `printObj` method on both objects and get the appropriate results.

```js
myVehicle.printObj() // "I drive a black 2017 Toyota Tacoma with AWD and tow package"
secondVehicle.printObj() // "I drive a Silver 2003 Toyota Rav4 with 5 speed manual transmission"
```

In this example, the concept of `this` is transfered to the scope of the newly created `secondVehicle` object even though it's original context was within the `myVehicle` object.

While the option to create a new object based in the prototype of an existing object is pretty cool, in most cases it's probably better to start out with a [constructor](/thisConstructor.md) as not to create this dependency chain.

Alternatively we can abstract the `printObj` method all together by going slightly functional over OOP.

```js
function logString() {
  console.log(`I drive a ${this.color} ${this.year} ${this.make} ${this.model} with ${this.options}`)
};
```

Then in our object, we can still have the key of `printObj`, but I assign the function of `logString`.

Notice, and this is important, I am adding `logString` and not `logString()`, this is not a typo. We want the **function itself** to be bound to the key, not the **function's result**. This is necessary as the `this` property really has no context within the scope of this function. It needs to be bound to an object in order for it to return a result.

```js
const myVehicle = {
  make: 'Toyota',
  model: 'Tacoma',
  color: 'black',
  year: 2017,
  options: 'AWD and tow package',
  printObj: logString,
}

const secondVehicle = {
  make: 'Toyota',
  model: 'Rav4',
  color: 'Silver',
  year: 2003,
  options: '5 speed manual transmission',
  printObj: logString,
}
```

At this point I can run this method on both objects and the value of `this` will be the context of the object itself.

```js
myVehicle.printObj();// "I drive a black 2017 Toyota Tacoma with AWD and tow package"
secondVehicle.printObj(); // "I drive a Silver 2003 Toyota Rav4 with 5 speed manual transmission"
```
