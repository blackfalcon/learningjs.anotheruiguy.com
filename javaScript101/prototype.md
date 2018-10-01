> {{book.title}} v{{book.version}}

# Inheritance and the prototype chain

Developers coming to JavaScript from other languages are quickly confused by the architecture of the language. JavaScript is NOT a class-based language like Java or C++. Even worse, it is a dynamically typed language and does not offer a class construct. And no, the `class` keyword introduced in ES6 is not a true class as described in other languages.

Experience has taught that when trying to understand JavaScript's prototype chain, one has to try not to draw a 1:1 comparison between JavaScript and other languages. Keep in mind that JavaScript has evolved over the years and has taken a lot of influence from other languages. To that end, JavaScript can be OOP or purely functional. It's not a directive of the language, but a personal/team style.

In addition to that, it is important to remember that inheritance and the prototype chain in JavaScript are not at all like class inheritance in other languages. There is no process of instantiation, but more simply, each object is referencing its ancestral prototype.

JavaScript has a single construct: objects. Digging deeper into the language one will quickly discover that even primitive data types are wrapped in objects. It's only `null` and `undefined` that have no object context as they are void of data. Every object has a private property that contains a link back to its prototype. These property links continue to reference its prototype until an object is reached with `null` as its prototype. `null` has no prototype and is the final link in the chain.

To really understand this, one needs to understand how an object works. The simplest definition of an object is that it has properties and methods. Take for example the `string` primitive. This data type is wrapped in the `string` object which has a single property and hosts a series of methods that help developers understand the properties of the string.

```js
const str = "Learning JavaScript can be fun and exciting!"

// string.length is a property of the string object
console.log(str.length)

// chaining together a series of methods allows for interesting ways to play with data
console.log(str.slice(0, 19).concat(' is awesome!').toUpperCase()) // "LEARNING JAVASCRIPT IS AWESOME!"
```

## Inheritance via constructors

A common method of creating custom prototypes is through the use of constructors. A constructor in JavaScript is simply a function that is called with the `new` operator. In this example, each new reference to `Person` will inherit the constructor's properties and methods. By supplying default values on the parameters of the constructor, not only are the properties and methods inherited but the property's values as well.

```js
const Person = function(height = '3 feet, 1 inch', hairColor = 'green') {
  this.height = height
  this.hairColor = hairColor
  this.str = function() {
    return `This person is ${this.height} with ${this.hairColor} hair.`
  }
}

const father = new Person('5 feet, 9 inches', 'gray')
const mother = new Person('4 feet, 10 inches', 'black')

// placing arguments into an array bound to a variable
const son = ['3 feet, 6 inches', 'brown']

// using the spread syntax to parse the items of the son array
const child = new Person(...son)

const alien = new Person()
// replacing the default str method with a new one for this instance, all other methods are still inherited
alien.str = function() {
  return `This alien is ${this.height} tall with ${this.hairColor} skin.`
}

console.log(father.str()) // "This person is 5 feet, 9 inches with gray hair."
console.log(mother.str()) // "This person is 4 feet, 10 inches with black hair."
console.log(child.str()) // This person is 3 feet, 6 inches with brown hair."
console.log(alien.str()) // "This alien is 3 feet, 1 inch tall with green skin."
```

As illustrated, with each instance of the `Person` constructor object, the default properties and methods are always available. By passing new arguments into the parameters of the constructor function, this over-rides the defaults and outputs a custom string. But even notice how `alien.str` will override the method in the constructor function.

With each execution of the method, the function will look all the way back to its original prototype for information that is not directly applied in its instance.

## Set the inheritance of an object to a prototype object

Creating new objects that directly inherit properties from a constructor function is pretty straightforward. But there will be cases where functionality requires that a new object is created based on a previously created object and abstracting to a constructor function is overkill.

The following example illustrates two individual objects. The goal here is to combine these two individual objects into one and leverage the method in the first object.

```js
const car = {
  make: 'Toyota',
  printObj: function() {
    console.log(`${this.make} ${this.model}`)
  }
}

const myCar = {
  model: 'Tacoma'
}
```

When abstraction is not an option, using the `setPrototypeOf()` method may be an option. The purpose of this method is to bind the `myCar` object to the `car` object as its prototype.

```js
Object.setPrototypeOf(myCar, car)
console.log(myCar)
```

Viewing this console.log, you will see that the two objects are now combined to one.

```js
[object Object] {
  make: "Toyota",
  model: "Tacoma",
  printObj: function() {
    window.runnerWindow.proxyConsole.log(`${this.make} ${this.model}`)
  }
}
```

Running the next line of code, you will get the output of the `.printObj` method.

```js
myCar.printObj() // "Toyota Tacoma"
```

> Warning: Changing the [[Prototype]] of an object is, by the nature of how modern JavaScript engines optimize property accesses, a very slow operation, in every browser and JavaScript engine. The effects on performance of altering inheritance are subtle and far-flung, and are not limited to simply the time spent in Object.setPrototypeOf(...) statement, but may extend to any code that has access to any object whose [[Prototype]] has been altered. If you care about performance you should avoid setting the [[Prototype]] of an object. Instead, create a new object with the desired [[Prototype]] using Object.create().

## Objects inherit properties and methods of other objects

The previous example is a process of assigning the prototype of an object to another object in a manner that could be viewed as a backward action. It is always, as per performance reasons explained above, to keep things going in a forward manner.

In this example, the object of `car` is created using the Object Literal syntax. Using the Object.assign() and Object.create() methods, the new object `van` can be created from `car` and will directly inherit its properties and methods. Aside from the lack of a constructor function, all the same rules apply in regards to prototypal inheritance.

Note that the `this` property is maintained within the context of the individual object.

```js
const car = {
  color: 'black',
  doors: 4,
  whatCar: function() {
    console.log(`${this.color} car with ${this.doors} doors`)
  }
}

car.whatCar(); // "black car with 4 doors"

// this statement will assign color: 'orange' to the
// newly created 'van' object based on the 'car' prototype
const van = Object.assign(Object.create(car), {
  color: 'orange'
})

van.whatCar(); // "orange car with 4 doors"
console.log(van.color) // "orange"
```
