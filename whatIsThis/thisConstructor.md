> {{book.title}} v{{book.version}}

# Within a Constructor

Constructors in JavaScript are pretty powerful tools, and we will cover more of this in greater detail later on, but in this section let's talk more about how the `this` property works inside a Constructor function.

In all things, templates, molds, what have you, once you have defined a pattern of use, there is typically a tool available to repeat that pattern again and again without having to build from scratch. In JavaScript, the tool is called a constructor.

Constructors are templates for objects. Let's imagine that we have a program where we will be defining a lot of cars. It would take additional effort to always have to create each car object from scratch. So to templatize this process, I can create a constructor.

In our example, I will create a constructor called `Automobile`. This constructor will consist of four properties,  `make`, `model`, `color`, and `year`. The `Automobile` constructor will look like the following example.

```js
function Automobile(make, model, color, year) {
  this.make = make,
  this.model = model,
  this.color = color,
  this.year = year
}
```

What's important to notice here is that `this` does NOT refer to the `Automobile` constructor itself. After all, the `Automobile` constructor is void of data. It's only purpose right now is to be a placeholder for future data.

With each use of the `Automobile` constructor,  `this` will refer to the data associated with the new object. In the following example, I will create two new objects using this constructor.

The syntax to create a new object based off of a constructor is to declare the new object, `const myCar` in this example, using the `new` keyword to create a new object from the `Automobile` constructor.

You should also notice that capitalization of the constructor name. This is a convention that most JavaScript developers use so that it's easy to see the difference between a standard function and a constructor function.

```js
const myCar = new Automobile('Toyota', 'Tacoma', 'black', 2017);
const familyCar = new Automobile('Toyota', 'Sienna', 'black', 2008);
```

Logging these two new objects in the console will show how `this` was applied to the properties of the object.

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

To really illustrate that the `this` property is NOT referencing the constructor, but the new object that is created from the constructor, I will create a new print method on the object.

Remembering back to our talk on [Objects and Methods](/objectsMethods.md), using the `this` property, we created an object with a method and either cloned that object or abstracted the method into an external utility function that was applied as a method function within the context of the object. While these techniques will work, you can create awkward dependency chains.

Knowing what we know about constructors and methods, instead of manually doing this work, let's build a method in the context of the constructor and apply that method universally to any new object we create from that constructor. This constructor will be the inherited prototype for each newly created object.

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

With our method inside the constructor, the objects we created will automatically inherit this method, removing the need to write any other code. We can now use the `printObj` method on the new objects as shown in the example below.

```js
myCar.printObj(); // "I drive a black 2017 Toyota Tacoma"
familyCar.printObj(); // "I drive a black 2008 Toyota Sienna"
```

Also note that since the `Automobile` constructor is the individual object's prototype, if this constructor is updated, the decedent objects will be updated as well.

## Constructor or Class?

In ES6 JavaScript Classes were introduced. It should be said that Classes really are, more or less, syntactic sugar on top of traditional JavaScript Constructors. It is important to note that **Class Declarations**, unlike **Function Declarations**, are **NOT** hoisted. Meaning, that you must define your class before referencing it.

```js
var papa = new Person(); // ReferenceError

class Person { ... }
```

The primary parts of a class include the `constructor` and then `methods`. Some interesting things to note is; classes themselves do not have parameters, classes maintain the scope of `this` just like any other constructor, the `function` keyword is not used, you cannot use the arrow syntax `=>`.

```js
class foo {
  constructor() {
    ...
  }

  method() {
    ...
  }
}
```

Let's rebuild our previous example as a class versus a constructor so that you can see the differences. See in the following example that with a class you use the `class` keyword followed by its name and no parens `()` for arguments. Just like constructors, naming convention is to capitalize the fist letter and CamelCase the rest.

Nested is the `constructor` function. No need for the `function` keyword here and be sure to pass in your parameters for the object.

Next is the method(s). Again, no need for the `function` keyword and set up any parameters needed.

```js
class Automobile {
  constructor(make, model, color, year) {
    this.make = make,
    this.model = model,
    this.color = color,
    this.year = year
  }

  printObj() {
    console.log(`I drive a ${this.color} ${this.year} ${this.make} ${this.model}`)
  }
}
```

Using this new class and its method(s) is no different than using the previous constructor. In fact, this is really helpful as you can refactor old constructors to use classes and not have to be worried about breaking the use of the older constructors in your app.

```js
const myCar = new Automobile('Toyota', '4-Runner', 'black', 2019)

myCar.printObj(); // "I drive a black 2019 Toyota 4-Runner"
```

And like I said, a class is mostly syntactic sugar on top of a constructor, so this syntax style has no baring on how `this` is applied or extended. For example, if I were to assign the `printObj()` function to another variable, it would require that I use the [.call() or .apply()](/callApply.md) methods in order to retrieve the values of the object so that `this` has context.

Illustrated below I am creating a new variable `printCar` and to that variable I want to assign the `myCar.printObj` method. The problem here is that when I try to use the method it has lost all context with any data, so `this` has no value. The way to address this is to bind the data from some other object to this method call. In this case, I am binding the data from the `myCar` object.

```js
const printCar = myCar.printObj;

printCar.call(myCar); // "I drive a black 2019 Toyota 4-Runner"
```

Remember, the [=>](arrowFunctions.md) syntax will not work in a class. If this is confusing, don't worry, we cover more on [.call() or .apply()](/callApply.md) methods in the next section.

## Extending a Class

With the advent of ES6 and the new Class special function also came along a really amazing simple way to take the concept of a constructor and extend it so that the original properties of the constructor could be passed onto a new constructor.

For example, take the preceding Class of Automobile. On the surface this is a pretty simple object being created and these properties are not specific to an automobile. What if you wanted to build a similar object, except this would be used for a motorcycle and the only difference is that the output would be `ride` versus `drive`. This can easily be accomplished with the new Class special function.

The example below is creating a new constructor called `Motorcycle` and updating the `printObj()` method within has the updated string. Using the `extends` keyword I am creating a new class of `Motorcycle` that is extending all the properties of `Automobile`. Then inside that new Class I am redefining the `printObj()` method with the updated string.

```js
class Motorcycle extends Automobile {
  printObj() {
    console.log(`I ride a ${this.color} ${this.year} ${this.make} ${this.model}`)
  }
}
```

Now to use this new Class I will create a new object for `myBike` like so.

```js
const myBike = new Motorcycle('Harley Davidson', 'Soft-tail', 'black and chrome', 2009)
```

To see the results of this extended Class, I run the following.

```js
myBike.printObj(); // "I ride a black and chrome 2009 Harley Davidson Soft-tail"
```

As you can see here, the concept of `this` is pretty interesting. Without copying any of the `this.*` properties from the original `Automobile` Class, I am able to inherit those same properties and the concept of `this` stays within the context of the extended Class.

If you want to see something interesting, check out this [ES5 version](https://babeljs.io/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=MYGwhgzhAECCCuAXA9gW2QIwJYgKbQG8AoaaYZAOwkQCd5gUaAKVMAa1wBpp0ATXEN3IhkNbgE9cYGgEpCJUtEQALLBAB0rDtAC8PdlwWkVazcn4hdPcwM5GlqjcNFXnY-yY2TpV7zQUAvkQKAA40WBSIAPIYAFZMcsSKZJQQyHjqIgDmTAAGAJLQvOEAbvhg0AAkBJ7qbgFVNY7qfg3VtVq4bU2mfAIBuTKBREFEoJAwALLIjMDioPi4AB6IuBS8MAgo6Nh48qRhEdFxCfuK5FTpuJnIOQXQ4fzQFe3N9Y21rR_Nnd0dNiABkNSEFRhdqDxxABhHx6Ci4ADucCQaEwOFwTAA5AAVZDiGZgTHcTEAFgAtAAleAUeE0InQTEYcDANj0gBMAAYAIwATiG4MQkIAQlhtHDEdBprN5ngsQAJaR4cTQAAiYBKWA2lHpmIAysgAGaIMmIMA4HVMsAs57rMjKGhoXDsjkcvnBVDQ6TqQ6RGLxGQAbmgAHpg9AAESFYpYMrPaCW62c3nQXH403QclUmm4GjhogekUcb3hX0nQNAA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=false&targets=&version=6.26.0&envVersion=) of the same code. It's damn painful!
