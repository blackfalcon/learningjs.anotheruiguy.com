# Within a Constructor

Constructors in JavaScript are pretty powerful tools, and I go into greater details [later on](/thingsToKnow/constructors.html), but in this section let's talk more about how the `this` property works inside a Constructor function.

Constructors are templates for object creation. Let's imagine that we have a program where we will be defining cars on a lot. It would take additional effort to always have to create these car objects from scratch. Since we have computers we can make a template object and use that to create additional objects.

Out Constructor could look something like the following example.

```js
function Automobile(make, model, color, year) {
  this.make = make,
  this.model = model,
  this.color = color,
  this.year = year
}
```

What's important to notice here is that `this` does NOT refer to the `Automobile` constructor, but `this` will refer to the new object that is created from this constructor. In the following example I will create two new objects using this constructor.

```js
const myCar = new Automobile('Toyota', 'Tacoma', 'black', 2017);
const familyCar = new Automobile('Toyota', 'Sienna', 'black', 2008);
```

When we log these two new objects in the console we will see how `this` was applied to the properties of the object.

```js
[object Object] {
  color: "black",
  make: "Toyota",
  model: "Tacoma",
  year: 2017
}
[object Object] {
  color: "black",
  make: "Toyota",
  model: "Sienna",
  year: 2008
}
```

To really illustrate that the `this` property is NOT referencing the constructor, but the new object that is created from the constructor, we can create a print method on the object.

Remembering back to the section on [methods and objects](/thingsToKnow/thisMethod.html) using the `this` property, we manually created two objects and then a utility function that was applied as a method function within the context of object. Knowing what we know about constructors and methods, instead of manually doing this work, let's build a method in the context of the constructor and apply that method universally to any new object we create from that constructor.

First, let's update the constructor.

```js
function Automobile(make, model, color, year) {
  this.make = make,
  this.model = model,
  this.color = color,
  this.year = year
  this.printObj = function() {
    console.log(`I drive a ${this.color} ${this.year} ${this.make} ${this.model}`)
  }
}
```

With our method inside the constructor, the objects we created will automatically inherit this method removing the need to write any other code. We can now use the `printObj` method on the new objects as shown in the example blow.

```js
myCar.printObj(); // "I drive a black 2017 Toyota Tacoma"
familyCar.printObj(); // "I drive a black 2008 Toyota Sienna"
```

To see this in action, check out [this JSBin](https://jsbin.com/sovozod/edit?js,console).
