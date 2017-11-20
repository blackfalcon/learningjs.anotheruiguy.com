# Object-oriented programming (OOP)

> A programming paradigm based on the concept of "objects", which may contain data, in the form of fields, often known as attributes; and code, in the form of procedures, often known as methods.

The rabbit hole of OOP gets really deep, really fast, so we aren't going to go down that path. But, what we can do is really scratch hard at the surface of what it means to develop your JavaScript in a OOP paradigm. Simply put, the idea of OOP is to encapsulate your code concepts figuratively and problematically. Let's imagine that we work in the DMV and we have to build some software that will allow a user of our app to register new car tabs and even renew those tabs into the future.

The up side to this is code is centrally located and helps to make your code library easy to navigate. Reading this you should be saying to yourself, "_Yes, this is very object oriented!_" And you would be correct. Everything starts and ends with the object.

The down side to this is all your code is centrally located. I've seen this abused more in Java where there are classes that are HUGE and extremely hard to follow and even harder to refactor. In the end, there is no _right_ way to do these things and in some cases, regardless of the functional programming hype, using the OOP paradigm is the right way to go!

## Let's dig into some code

In this example we are going to create a constructor that we can use to build the data object and then using an inheritance model, the core functionality of the object will be passed into each object in order to perform its functions. Many like this option for programming as this clearly encapsulates the features with the data and it is very state aware.

To start out we need to build our `Automobile`constructor that will take a small handful of arguments to define the initial properties of this object.

```js
function Automobile(make, model, color, year) {
  this.make = make,
  this.model = model,
  this.color = color,
  this.year = year,
}
```

Nothing terribly new here, so let's go to the next step, the functions/methods for the objects created from this constructor. One of the core features of our car tab app is that we need to print (`printObj()`) out the state of the user's registration. Note that I am checking for `tabsExp`. This is not a parameter of the Automobile constructor? What this is stating is that if the vehicle has yet to be registered there is no `tabsExp` data and if that is so, print that statement. Otherwise, print the other statement.

```js
this.printObj = function() {
  if (this.tabsExp === undefined) {
    console.log(`I purchased a ${this.color} ${this.year} ${this.make} ${this.model}. I need to register with the DMV.`)
  } else {
    console.log(`I own a ${this.year} ${this.make} ${this.model}, the tabs expire in ${this.tabsExp}`);
  }
}
```

Now that we can print the state of the registration, the next action someone will need to do when registering a new vehicle is purchase new tabs. We'll call this our `newTabs` method. This method will get the current date and output a string stating the vehicle's info and the assumed expiration date of the car tabs. Notice it's in this method that we are setting the `tabsExp` property. Then we print something back to the console that this action has happened. Feedback to the human is a good thing.

```js
this.newTabs = function() {
  const today = new Date();
  const thisYear = today.getFullYear();

  this.tabsExp = thisYear + 1;
  console.log(`I purchased new tabs for my ${this.year} ${this.make}. They expire in ${this.tabsExp}.`)
}
```

The last feature we want to add to this app is the ability for someone to renew their tabs for another year. This will be our `renewal` method. This is pretty straight forward, we go into the object, fine the `tabExp` value and add `1`, then log a statement back to the console.

```js
this.renewal = function() {
  this.tabsExp = this.tabsExp + 1;
  console.log(`I renewed my tabs, they expire in ${this.tabsExp}.`);
}
```

There you have it, a fully featured constructor that consists of three methods that will perform actions on the object created from this constructor. Just so that you can see it all in one place.

```js
function Automobile(make, model, color, year) {
  this.make = make,
  this.model = model,
  this.color = color,
  this.year = year,

  this.newTabs = function() {
    const today = new Date();
    const thisYear = today.getFullYear();

    this.tabsExp = thisYear + 1;
    console.log(`I purchased new tabs for my ${this.year} ${this.make}. They expire in ${this.tabsExp}.`)
  },

  this.renewal = function() {
    this.tabsExp = this.tabsExp + 1;
    console.log(`I renewed my tabs, they expire in ${this.tabsExp}.`);
  },

  this.printObj = function() {
    if (this.tabsExp === undefined) {
      console.log(`I purchased a ${this.color} ${this.year} ${this.make} ${this.model}. I need to register with the DMV.`)
    } else {
      console.log(`I own a ${this.year} ${this.make} ${this.model}, the tabs expire in ${this.tabsExp}`);
    }
  }
}
```

Looking at this, we can see the ups and downs to this style of code. All the methods are in one place, but this constructor will grow with each added feature, guaranteed.

Now, how do we use this new code? Well, first we need to instantiate our new object `familyCar`.

```js
const familyCar = new Automobile('Toyota', 'Sienna', 'Black', 2008);
```

Now that we have our object, we can easily run our methods and produce the output we are looking for.

```js
familyCar.printObj(); // "I purchased a Black 2008 Toyota Sienna. I need to register with the DMV."
familyCar.newTabs(); // "I purchased new tabs for my 2008 Toyota. They expire in 2018."
familyCar.printObj(); // "I own a 2008 Toyota Sienna, the tabs expire in 2018"
familyCar.renewal(); // "I renewed my tabs, they expire in 2019."
familyCar.printObj(); // "I own a 2008 Toyota Sienna, the tabs expire in 2019"
```

The code pretty much speaks for itself. Everything starts from the object's perspective. What is the object and what is the method I will use to create, read, update, delete its data.

## How some things compare

Object-oriented programming is diametrically opposed to [Functional programming](functional.md) and to the same extent [Procedural programming](procedural.md) because OOP bundles the data and methods together, so an _object_, which is an instance of a constructor, operates on its _own_ data structure.
