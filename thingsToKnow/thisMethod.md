# Within Methods and Objects

As we learned in the explanation of [objects](/thingsToKnow/objects.html), objects can have properties and methods can be applied to them to expose their data. You can also define a new method within the scope of your object. This is important to understand because there are implications to the `this` property as well.

To start out, let's make a standard object that describes the `myVehicle` variable.

```js
const myVehicle = {
  make: 'Toyota',
  model: 'Tacoma',
  color: 'black',
  year: 2017,
  options: 'AWD and tow package'
}
```

For our method, we can create that directly within the context of the object itself as illustrated in the following updated example.

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

What's important to notice is that the `printObj` method is bound to the `myVehicle` object directly by context. To get this to execute, we call the object and it's method which will return the string we are creating.

```js
myVehicle.printObj(); // "I drive a black 2017 Toyota Tacoma with AWD and tow package"
```

To make this a little more interesting, consider that we have not only `myVehicle`, but `secondVehicle` as well. Now I'd hate to have to create this method twice, so let's extract the nested method function and make this a globally accessible function.

```js
function logString() {
  console.log(`I drive a ${this.color} ${this.year} ${this.make} ${this.model} with ${this.options}`)
};
```

Then in our object we can still have the key of `printObj`, but we assign the function of `logString`. Notice that we are adding `logString` and not `logString()` as we want the function itself to be bound to the key and not the function's result.

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

Now we can run this method on both objects and the value of `this` will be the context of the object itself.

```js
myVehicle.printObj();// "I drive a black 2017 Toyota Tacoma with AWD and tow package"
secondVehicle.printObj(); // "I drive a Silver 2003 Toyota Rav4 with 5 speed manual transmission"
```

To see this in action, check out [this JSBin](https://jsbin.com/wihawuc/edit?js,console).
