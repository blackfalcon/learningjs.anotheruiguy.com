> {{book.title}} v{{book.version}}

## argsArray
An array-like object, specifying the arguments with which `func` should be called, or `null` or `undefined` if no arguments should be provided to the function.

## Camel case
Also known as camel caps or more formally as medial capitals) is the practice of writing compound words or phrases such that each word or abbreviation in the middle of the phrase begins with a capital letter, with no intervening spaces or punctuation.

```js
function Name() {
  ...
}

function NewName() {
  ...
}
```

## Coercion
The conversion of values into common data types to facilitate operations like comparison. For more, in-depth reading on this subject, please see [All you need to know about Type coercion in JavaScript](http://idiallo.com/javascript/type-coercion-conversion-in-javascript).

## CoffeeScript
A programming language that transcompiles to JavaScript. It adds syntactic sugar inspired by Ruby, Python and Haskell in an effort to enhance JavaScript's brevity and readability. Specific additional features include list comprehension and pattern matching.

## Conditional (ternary) Operator
The conditional (ternary) operator is the only JavaScript operator that takes three operands. This operator is frequently used as a shortcut for the `if()` statement.

```js
condition ? expr1 : expr2
```

If condition is true, the operator returns the value of `expr1`; otherwise, it returns the value of `expr2`.

## DOM
The DOM is a document model loaded in the browser and representing the document as a node tree, where each node represents part of the document (e.g. an element, text string, or comment).

## DRY
In software engineering, don't repeat yourself (DRY) is a principle of software development aimed at reducing repetition of software patterns, replacing them with abstractions; and several copies of the same data, using data normalization to avoid redundancy.

## ECMAScript
The JavaScript standard

## Enumerable properties
Enumerable properties are those properties whose internal [[Enumerable]] flag is set to true, which is the default for properties created via simple assignment or via a property initializer (properties defined via `Object.defineProperty` and such default [[Enumerable]] to false).

Enumerable properties should not be confused with enums (enumerations), which is not natively supported in JavaScript. Read [Using enums (enumerations) in javascript](https://www.sohamkamani.com/blog/2017/08/21/enums-in-javascript/) to learn more.

## ES6
ECMAScript 2015 is the sixth edition of the ECMAScript Language Specification standard.

## First-class functions
In computer science, a programming language is said to support first-class functions (or function literal) if it treats functions as first-class objects. Specifically, this means that the language supports constructing new functions during the execution of a program, storing them in data structures, passing them as arguments to other functions, and returning them as the values of other functions.

## First-order function
A function that does not take a function as an argument or return a function as output.

## Function
Generally speaking, a function is a "subprogram" that can be called by code external (or internal in the case of recursion) to the function. Like the program itself, a function is composed of a sequence of statements called the function body. Values can be passed to a function, and the function will return a value.

In JavaScript, functions are first-class objects, because they can have properties and methods just like any other object. What distinguishes them from other objects is that functions can be called. In brief, they are Function objects.

## Higher-order functions
Functions that do at least one of the following:

* takes one or more functions as arguments
* returns a function as its result

## Hoisting
Hoisting teaches that variable and function declarations are physically moved to the top of your code, but this is not what happens at all. Hoisting is a process in which the order of variables and declarations are loaded into the JavaScript engine.

```js
console.log(x); // -> undefined
var x = 5;
console.log(x); // -> 5
console.log(y); // -> error, no var declared
```

The JavaScript engine breaks apart the variable declaration and loads it into memory in this order.

```js
var x;
console.log(x); // -> undefined
x = 5;
console.log(x); // -> 5
console.log(y); // -> error, no var declared
```

## IIFE
Immediately-invoked function expressions can be used to avoid variable hoisting from within blocks, protect against polluting the global environment and simultaneously allow public access to methods while retaining privacy for variables defined within the function.

```js
(function name() {
  ...
})();
```

## Immutable
An immutable object is one whose content cannot be changed. [See article](https://slemgrim.com/mutate-or-not-to-mutate/)

## Imperative
A programming paradigm that uses statements that change a program's state. In much the same way that the imperative mood in natural languages expresses commands, an imperative program consists of commands for the computer to perform. Imperative programming focuses on describing how a program operates.

## Iterable objects
The iterable protocol allows JavaScript objects to define or customize their iteration behavior, such as what values are looped over in a `for ... of()` construct. Some built-in types are built-in iterables with a default iteration behavior, such as Array or Map, while other types (such as Object) are not.

## Lexical scope
A convention used with many programming languages that sets the scope (range of functionality) of a variable so that it may only be called (referenced) from within the block of code in which it is defined.

With lexical scope, a name always refers to its (more or less) local lexical environment. This is a property of the program text and is made independent of the runtime call stack by the language implementation. Because this matching only requires analysis of the static program text, this type of scoping is also called static scoping.

Correct implementation of static scope in languages with first-class nested functions is not trivial, as it requires each function value to carry with it a record of the values of the variables that it depends on (the pair of the function and this environment is called a closure).

## Method
A method is a function which is a property of an object. There are two kind of methods: Instance Methods which are built-in tasks performed by an object instance, or Static Methods which are tasks that can be performed without the need of an object instance.

They are essentially functions that are created with the scope of an object to be performed on that particular object or prototype. As shown below, a method is a function assigned to an object property.

```js
const foo = {
  color: 'orange',
  size: 'large',
  printObj: function() {
    console.log(`${this.color} and ${this.size}`)
  }
}
```

## Mutable
Mutable is a type of variable that can be changed. In JavaScript, only objects and arrays are mutable, not primitive values. [See article](https://slemgrim.com/mutate-or-not-to-mutate/)

## Mutating
Changing or affecting a source element; the goal is to keep the original element unchanged at all times, otherwise known as '_single source of truth_'.

> A mutation is a side effect: the fewer things that change in a program, the less there is to keep track of, which results in a simpler program.

## Object
An object is a collection of related data and/or functionality (which usually consists of several variables and functions — which are called properties and methods when they are inside objects.)

## Object.assign
The Object.assign() method is used to copy the values of all enumerable own properties from one or more source objects to a target object. It will return the target object.

Note: the example below illustrates that this function does not over-write the contents of the previous object when creating the new object.

```js
const weatherToday = {
  temp: '89º',
  skys: 'sunny',
  wind: 'calm'
};

const weatherTomorrow = Object.assign({temp: '80º', humidity: 'low'}, weatherToday);

console.log(weatherTomorrow.temp, weatherTomorrow.skys, weatherTomorrow.humidity); // "89º" "sunny" "low"
```

## Object.create
The Object.create() method creates a new object, using an existing object to provide the newly created object's [[Prototype]]. (see browser console for visual evidence.)

```js
const transportation = {
  person: '',
  isFast: true,
  printCommute: function () {
    console.log(`${this.person} ${this.mode} to get to work. ${this.isFast ? "It is pretty fast" : "It takes way too long"}`);
  }
};

const dad = Object.create(transportation);

dad.person = 'Dad';
dad.mode = "takes the train"; // "name" is a property set on "me", but not on "person"
dad.isFast = true; // inherited properties can be overwritten

const mom = Object.create(transportation);

mom.person = 'Mom';
mom.mode = "takes the bus";
mom.isFast = false

dad.printCommute();
mom.printCommute();
```

## Object.assign versus Object.create
The real main difference between `Object.assign()` and `Object.create` is that `Object.assign()` will assign new properties and methods to an existing object. Whereas `Object.create()` will create a whole new object using the previous object's prototype as the template.

## Object literal
An object initializer is a comma-delimited list of zero or more pairs of property names and associated values of an object, enclosed in curly braces `{}`.

```js
const thing = {
  color: 'orange',
  height: 'tall',
  age: 65
};
```

The object literal notation is **not** the same as the JavaScript Object Notation (JSON).

```json
[
 {
  "thing": {
    "color": "orange",
    "height": "tall",
    "age": 65
  }
 }
]
```

## Object literal
An object literal is a comma-separated list of name-value pairs wrapped in curly braces `{}`.

```js
var obj = {
  color: 'orange',
  qty: 2,
  inStock: false
};
```

## OOP
OOP (Object-Oriented Programming) is an approach in programming in which data is encapsulated within objects and the object itself is operated on, rather than its component parts.

JavaScript is heavily object-oriented. It follows a prototype-based model (as opposed to class-based).

## Object Oriented Programming
See OOP

## Procedural
Derived from structured programming, based upon the concept of the procedure call. Procedures, also known as routines, subroutines, or functions (not to be confused with mathematical functions, but similar to those used in functional programming), simply contain a series of computational steps to be carried out. Any given procedure might be called at any point during a program's execution, including by other procedures or itself. The first major procedural programming languages first appeared circa 1960, including Fortran, ALGOL, COBOL and BASIC.[1] Pascal and C were published closer to the 1970s, while Ada was released in 1980.[1] Go is an example of a more modern procedural language, first published in 2009.

## Pure functions
A pure function is a function that will

* Given the same input, will always return the same output
* Produces no side effects

> A dead giveaway that a function is impure is if it makes sense to call it without using its return value

## Runtime
The Runtime is code that runs our code. In NodeJs or the Chrome Browser, the V8 Engine is written in C++. See [here](https://www.eventedmind.com/classes/the-javascript-runtime/what-is-a-runtime) for a video that best explains the JavaScript Runtime.

## Strict
A feature introduced in ES5, tells the JavaScript interrupter to run in STRICT mode. This has no effect on the output of your code nor does it allow for different features to work. It simply changes how the interrupter will run your code and is typically used for better troubleshooting.

## Sass
Sass (syntactically awesome stylesheets) is a style sheet language initially designed by Hampton Catlin and developed by Natalie Weizenbaum. After its initial versions, Weizenbaum and Chris Eppstein continued to extend Sass with SassScript, a simple scripting language used in Sass files.

## Scope
The current context of execution. The context in which values and expressions are "visible," or can be referenced. If a variable or other expression is not "in the current scope," then it is unavailable for use. Scopes can also be layered in a hierarchy, so that child scopes have access to parent scopes, but not vice versa.

A function serves as a closure in JavaScript, and thus creates a scope, so that (for example) a variable defined exclusively within the function cannot be accessed from outside the function or within other functions.

## Silent failure
A case where the code fails to produce a return to the UI or console. In many cases, these failures are expected and no UI return is needed. In other cases, the silent failure is unexpected and troubleshooting can be frustrating.

## Symbol
The data type "**symbol**" is a primitive data type having the quality that, values of this type can be used to make object properties that are anonymous. The symbol data type is highly specialized in purpose, and remarkable for its lack of versatility.

When a symbol value is used as the identifier in a property assignment, the property (like the symbol) is anonymous; and also is non-enumerable.  Because the property is non-enumerable, it will not show up as a member in the loop construct `for ... in`.

## Syntactic sugar
Syntax within a programming language that is designed to make things easier to read or to express. It makes the language "sweeter" for human use: things can be expressed more clearly, more concisely, or in an alternative style that some may prefer.

## Temporal Dead Zone
Order of variables and references, outside the scope of hoisting, and the temporal dead zone.

## Transpiler
Also commonly referred to as transcompiler, is a source-to-source compiler. Commonly seen in JavaScript libraries like CoffeeScript or TypeScript, or CSS libraries like Sass.

## thisArg
The value of `this` provided for the call to `func`. Note that `this` may not be the actual value seen by the method: if the method is a function in non-strict mode code, `null` and `undefined` will be replaced with the global object, and primitive values will be boxed.

## Type coercion
When the operands of an operator are different types, one of them will be converted to an "equivalent" value of the other operand's type.

```js
var a = 0;
var b = false;

var test = a == b;

console.log(test) // true
```

## Variable
A variable is a named location for storing a value. That way an unpredictable value can be accessed through a predetermined name.

## Value mutation
See Mutable and Immutable

## Variable shadowing
In computer programming, variable shadowing occurs when a variable declared within a certain scope (decision block, method, or inner class) has the same name as a variable declared in an outer scope.

























