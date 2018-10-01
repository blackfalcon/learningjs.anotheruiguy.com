> {{book.title}} v{{book.version}}

# Arrow functions

The ES6 Arrow Function has many properties that are of interest to developers, but the one aspect of this function that we will briefly explore is its ability to maintain lexical scope when used with constructors.

In regards to `this`, fat arrow functions have their own ability to manage its lexical scope. What this means is, unlike standard functions, arrow functions do not have their own `this`. They always retain the scope from which they came.

Arrow functions also cannot be used as constructors, and unlike everything else in JavaScript, arrow functions do not have a prototype property.

```js
const thing = () => {}
const otherThing = function() {}

console.log(thing.prototype) // undefined
console.log(otherThing.prototype) // [object Object]
```

Now the management of `this` is a complex subject, and one of the more confusing uses of `this` is with constructor functions. Not so much in the assigning of properties with each new instance, but when referencing `this` within a prototype's method.

Let's say for example you have a constructor that has a single method for printing out the properties of the object in the console. Nothing special here.

```js
function Vehicle(make, model, color, year) {
  this.make = make,
  this.model = model,
  this.color = color,
  this.year = year,
  this.printObj = function() {
    console.log(`I drive a ${this.color} ${this.year} ${this.make} ${this.model}.`)
  }
}

const truck = new Vehicle('Toyota', 'Tacoma', 'black', 2018);
```

When you call the object directly and the method, you get the return you expect as this is all within the lexical scope.

```js
truck.printObj(); // "I drive a black 2018 Toyota Tacoma."
```

Now the problem comes in when you try to assign that function to another variable in an outer scope.

```js
const truckPrint = truck.printObj;
```

At this point, we know that the `printObj()` function is disconnected from its original scope and thus `this` is broken.

```js
truckPrint() // "I drive a undefined undefined undefined undefined."
```

If this is what we have to work with, we could use the `.call()` method to reference an outer object and call it into the scope of the function we are using.

```js
truckPrint.call(truck) // "I drive a black 2018 Toyota Tacoma."
```

Using the `.call()` method is perfectly valid, but there is a flaw here that can really lead to bad code and real bugs in the system. The `call()` method does not have special knowledge of the original scope of the method. You are simply redirecting data to this scope. For example, let's add another new Vehicle object.

```js
const car = new Vehicle('Ford', 'Mustang', 'red', 1966)
```

Using the same `truckPrint()` variable that is the `printObj()` method from the `truck` object, we can actually do this.

```js
truckPrint.call(car) // "I drive a red 1966 Ford Mustang."
```

There may be those who think that this is ok, but this breaks every rule for code clarity and maintainability. Using another object's methods for unintended use is an open invitation for broken code when the intended use of the method is changed.

This is where the fat arrow function really comes in to play and keeps things like this from happening. Remember, arrow functions do not have their own `this`. When using the fat arrow in an object's method, the method always remains connected to its original scope. In the following example I illustrate how we would update our constructor function to use a fat arrow.

```js
function Vehicle(make, model, color, year) {
  this.make = make,
  this.model = model,
  this.color = color,
  this.year = year,
  this.printObj = () => {
    console.log(`I drive a ${this.color} ${this.year} ${this.make} ${this.model}.`)
  }
}
```

The same way we did previously, we could assign the `printObj()` function to a new variable, but this time the scope of `this` will follow.

```js
const truckPrint = truck.printObj;
```

Now when we reference the new `truckPrint()` function, we no longer need to re-associate this function with object data.

```js
truckPrint() // "I drive a black 2018 Toyota Tacoma."
```

Even if I try to to pass in references to other objects, the value of `this` is not passed into this function.

```js
truckPrint(car) // "I drive a black 2018 Toyota Tacoma."
truckPrint.call(car) // "I drive a black 2018 Toyota Tacoma."
```
