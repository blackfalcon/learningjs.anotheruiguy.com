> {{book.title}} v{{book.version}}

# Constructor functions

Constructors are functions. Constructors are templates. Constructors are objects. Constructors are functions that template the creation of objects.

There is a preferred naming convention for constructors and that is to capitalize the first letter of the name, if not also CamelCase the name if there are multiple words.

A constructor follows the same syntax of a standard function, using the `function` keyword, followed by the name you are giving the constructor function and then the parameters to be passed into the function inside the parens `()`.

What makes a constructor unique is the assignment of properties based on the parameters input using the `this` keyword.

```js
function NewConstructor (param) {
  this.param = param;
}
```

Let's look at an example where we can make the best use of a constructor function. You have an app where you are tracking car data. In this app you allow your users to enter data about their car and this data is stored as objects in the system.

```js
function Vehicle(type, make, model, color, year) {
  this.type = type;
  this.make = make;
  this.model = model;
  this.color = color;
  this.year = year;
}
```

Using a constructor function is not much different from that of assigning the return of a function to a variable. What is different is the use of the `new` keyword prior to the naming of the constructor. This tells JavaScript to create a new instance from the constructor prototype.

```js
var myMinivan = new Vehicle('van', 'toyota', 'seinna', 'black', 2008);
```

At this point, we have constructed a new object and assigned those values to the variable object of `myMinivan`. What makes this helpful is that with each instance of building out a new Vehicle object, we do not need to go through the steps of building the object manually.

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

When using a constructor to create an object, this is not just simply an object literal, but it's the instance of a prototype that was created with the constructor. To test this, we use the `instanceof` operator to evaluate the presence of `constructor.prototype` in the new object's prototype chain. In this case, we will know that this is `true`.

But now that we know this, we also know that any variable object we create from this constructor is part of the constructor's prototype chain and we can use that to our advantage. In the following example, I am building out a new constructor, building a new object from the constructor, evaluating it and even manipulating it.

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

// console log the foo object in its current state
console.log(foo);

// log out if foo really is an instance of Vehicle
console.log(foo instanceof Vehicle);

// manipulating the Vehicle prototype and inserting the global var from before
Vehicle.prototype.wheelCount = wheelCount;

// creating a new variable object
var bar = new Vehicle('truck', 'ford', 'F150', 'red', 2012);


// console log out the current states of the foo and bar variable objects
console.log(foo);
console.log(bar);
```

As you can see from the output below, by making `Vehicle` a constructor and thus a prototypical object, we can append things to the prototype on the fly in the script that will impact existing and newly created object in the script.

Notice above we added a new `wheelCount` key.

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
  color: "red",
  make: "ford",
  model: "F150",
  type: "truck",
  wheelCount: 4,
  year: 2012
}
```

In the same way we added the new `wheelCount` key, we can also add a method to this object. We will talk more about this in the next section.

```js
// manipulating the Vehicle prototype and inserting a method function
Vehicle.prototype.printObj = function() {
  console.log(`I drive a ${this.year} ${this.color} ${this.make} ${this.model}`)
}

bar.printObj(); // "I drive a 2012 red ford F150"
```

This is simply scratching the surface of constructors, but suffice you can see how this is similar to other functions in JavaScript and what makes it different.
