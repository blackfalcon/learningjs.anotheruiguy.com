# Constructors

Constructors are not new to JavaScript, but have taken on new life in ES6 in regards to building `classes`. I speak to this more in the Deep Dive section later on.

So, without getting too deep into the rabbit hole, let's take into consideration a common use for constructors, and that is of easily building repeatable objects. For example, you have an app where you are tracking car data. In this app you allow your users to enter data about their car and this data is stored as objects in the system. To make this easy on ourselves, we would create a `Vehicle` constructor. In the following example you will see that we create this like any other function.

```js
function Vehicle(type, make, model, color, year) {
  this.type = type;
  this.make = make;
  this.model = model;
  this.color = color;
  this.year = year;
}
```

What makes this different is how it's used. The leverage a constructor you use the `new` keyword, the name of the constructor and the necessary arguments to build out the object.

```js
var myMinivan = new Vehicle('van', 'toyota', 'seinna', 'black', 2008);
```

At this point, we have constructed a new object and assigned those values to the variable object of `myMinivan`. What makes this helpful is that with each instance of building out a new Vehicle object, we don't need to go through the steps of building the object.

When we run this new variable through the console, we get the following output.

```js
[object Object] {
  color: "black",
  make: "toyota",
  model: "seinna",
  type: "car",
  year: 2008
}
```

When we use a constructor to create an object, this is not just simply an Object Literal, but it's the instance of a prototype that was created with the constructor. To test this, we use the `instanceof` operator to evaluate the presence of `constructor.prototype` in the new object's prototype chain. In this case, we will know that this is `true`.

But now that we know this, we also know that any variable object we create from this constructor is part of the constructor's prototype chain and we can use that to our advantage. In the following example I am building out a new constructor, building a new object from the constructor, evaluating it and even manipulating it. See all the fun things you can do in JavaScript O_O

```js
// global var that we can use later
var wheelCount = 4;

// build out the constructor
function Vehicle(type, make, model, color, year) {
  this.type = type;
  this.make = make;
  this.model = model;
  this.color = color;
  this.year = year;
}

// crate object variable
var foo = new Vehicle('mini van', 'toyota', 'seinna', 'black', 2008);

// console log the foo object in it's current state
console.log(foo);

// log out if foo really is a instance of Vehicle
console.log(foo instanceof Vehicle);

// manipulating the Vehicle prototype and inserting the global var from before
Vehicle.prototype.wheelCount = wheelCount;

// creating a new variable object
var bar = new Vehicle('truck', 'ford', 'F150', 'Red', 2012);


// console log out the current states of the foo and bar variable objects
console.log(foo);
console.log(bar);
```

As you can see from the output below, by making making `Vehicle` a constructor and thus a prototypical object, we can append things to the prototype on the fly in the script that will impact existing and newly created object in the script. Above we added a new `wheelCount` key, but we could also add a function there as well if needed that would act as a method on the object.

```js
[object Object] {
  color: "black",
  make: "toyota",
  model: "seinna",
  type: "mini van",
  year: 2008
}
true
[object Object] {
  color: "black",
  make: "toyota",
  model: "seinna",
  type: "mini van",
  wheelCount: 4,
  year: 2008
}
[object Object] {
  color: "Red",
  make: "ford",
  model: "F150",
  type: "truck",
  wheelCount: 4,
  year: 2012
}
```

This is simply scratching the surface of constructors, but suffice you can see how this is similar to other functions in JavaScript and what makes it different. Also at this time I am not going to go down the rabbit hole of Constructors 2010, also known as `Object.prototype.constructor` which is still defined as ...

> Returns a reference to the Object constructor function that created the instance object.

But with ES6 and the introduction of JavaScript Classes, we also now have the [constructor method](/deepDive/classesAndConstructors.html).
