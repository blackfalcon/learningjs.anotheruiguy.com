> {{book.title}} v{{book.version}}

# Constructors, Classes, and Factory functions

The more that is learned about JavaScript, the more it is learned that the language comes down to a few basic components, one of those being functions. Functions, being first-class citizens, causes a lot of confusion to new devs simply because of the complexity in understanding the different ways functions are used in efforts to better manage issues of `this`, achieve code clarity and address micro issues in regards to performance.

Take, for example, constructors, classes and factory functions. In the end, all are functions. But what makes them different? To start, here is a small example of a constructor that could be used again and again to output new objects needed for an app to run. This will be the baseline code used in the following examples to draw comparisons.

```js
function Vehicle(type, make, model, color, year) {
  this.type = type;
  this.make = make;
  this.model = model;
  this.color = color;
  this.year = year;
  this.printObj = function() {
    return `I drive a ${this.year} ${this.color} ${this.make} ${this.model}`
  }
}
```

Leveraging this constructor, the following code would be supported in an app.

```js
const myMinivan = new Vehicle('van', 'toyota', 'seinna', 'black', 2008);

console.log(myMinivan.printObj())

const click = document.getElementById('click-me');
click.addEventListener('click', function() {
  this.innerHTML = myMinivan.printObj()
})
```

## ES6 Classes

It has been said time and time again that JavaScript is NOT a class-based language. It is a prototype-based language and the inclusion of Classes in ES6 is simply syntactic sugar on top of the existing function construct of ... constructors.

In a class, the syntax is not all that dissimilar to that of a constructor, aside from the point that method is not simply an anonymous function on a property. Instead, a class will break all the associated parts of a constructor into individual parts and this can add some code clarity. Notice that the arguments for this class are NOT on the `class` keyword, but are part of the `constructor` function nested within the class. Also, notice that `printObj()` is simply floating within the scope of the class. This is literally no different than `this.printObj = function(){}`.

Also, keep in mind that when creating a new object from this class, this not instantiating a new instance of the class. The new object inherits its prototype ancestry as with a more traditional constructor.

```js
class Vehicle {
  constructor(type, make, model, color, year) {
    this.type = type;
    this.make = make;
    this.model = model;
    this.color = color;
    this.year = year;
  }

  printObj() {
    return `I drive a ${this.year} ${this.color} ${this.make} ${this.model}`
  }
}
```

Leveraging this class, the exact same code would be supported in an app.

```js
const myMinivan = new Vehicle('van', 'toyota', 'seinna', 'black', 2008);

console.log(myMinivan.printObj())

const click = document.getElementById('click-me');
click.addEventListener('click', function() {
  this.innerHTML = myMinivan.printObj()
})
```

## Factory functions

In code, not all scenarios are alike. Given a different context, the previous code solutions may cause issues related to the scope and how `this` works. Hacks, like using the `bind()` method or shoving in fat arrows, may help. Another possible solution is the concept of a factory function.

This is not a constructor or a class, so the naming convention is that of a traditional function. Also, notice the removal of the `this` keyword. The idea here is that all the variables are created within the scope of the function and directly returned when referencing of the `printObj()` method.

```js
const vehicle = (vehicleMake, vehicleModel, vehicleColor, vehicleYear) => {
  const make = vehicleMake
  const model = vehicleModel
  const color = vehicleColor
  const year = vehicleYear

  return {
    printObj: () => `I drive a ${year} ${color} ${make} ${model}`
  }
}
```

It should also be noted that using this syntax, there is no object created that is directly accessible. All that is returned from this function is the printed statement.

```js
console.log(myMinivan)
```

returns

```js
[object Object] {
  printObj: () => `I drive a ${year} ${color} ${make} ${model}`
}
```

In the execution of this factory function, the process is not dissimilar from the previous examples, but keep in mind that this code is not using the `new` keyword and as result, the new `myMinivan` object does not inherit its prototype from `vehicle`.

```js
const myMinivan = vehicle('toyota', 'seinna', 'black', 2008);

console.log(myMinivan.printObj())

const click = document.getElementById('click-me');
click.addEventListener('click', function() {
  this.innerHTML = myMinivan.printObj()
})
```
