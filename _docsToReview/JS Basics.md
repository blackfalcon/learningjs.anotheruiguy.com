# JS Basics

## Operators 

* `+=` is short hand for `variable = variable + "string";`

## Primitive

Data that is `not` an object and `has no methods`. 

There are 6 primitive data types: 

* string
* number
* boolean
* null
* undefined
* symbol (new in ECMAScript 2015).

## Falsy values

A falsy value is a value that translates to false when evaluated in a Boolean context.

* false
* 0 (zero)
* "" (empty string)
* null
* undefined
* NaN (a special Number value meaning Not-a-Number!)

## What is NULL

Intentional `absence` of any object value.

## Objects
* Objects are things that have properties and methods 
* a string is an object in JS, it's prototype and `length`
	* `'this is a javascript string'.length`
* Methods are actions you can perform against an object. 
	* `.split()` is a method you can perform on a string to separate into substrings(array) for processing
	* `var myObject =  "this is a string".split(' ');` == `["this", "is", "a", "string"]`
* Getting the property of an object post a method call
	* `var myObject =  "this is a string".split(' ').length;` == `4`

## Scoping of variables

Scoping is a pretty simple concept. Values start in the GLOBAL SCOPE and within each closing function, the scope becomes more defined. 

Within a function, the engine will look in it's current scope for a value, if not found it will continue to look outward into parent scopes until it resolves or get a reference error. Take the following example 

```
var x = 5;
var y = 10;

function fn() {
    var y = 20;
    console.log(x);
    console.log(y);
}

fn();
// -> 5
// -> 20

console.log(x); // -> 5
console.log(y); // -> 10
```

If you were to write this as ES6, it would be 

```
const x = 5;
const y = 10;

function fn() {
    const y = 20;
    console.log(x);
    console.log(y);
}

fn();
// -> 5
// -> 20

console.log(x); // -> 5
console.log(y); // -> 10
```

Something else that is interesting, if you were to do this

```
let x = 5;
let y = 10;

function fn() {
    y = 20;
    console.log(x);
    console.log(y);
}

fn();
// -> 5
// -> 20

console.log(x); // -> 5
console.log(y); // -> 20
```

But why? If you don't use `var` or `let`, the variable bubbles up through the layers of scope until it encounters a variable by the given name or the global object (window, if you are doing it in the browser), where it then attaches in the global scope. 

Very dangerous! 

## Functions and Methods 
Functions and methods both are functions in JavaScript. A **method** is a function which is a property of an object.

There are many pre-made methods in JavaScript, but you can also create custom methods on objects by extending the prototype. 

In the example below I am creating a constructor to crate a new object called 	`Person`. The constructor is helpful as anytime I use the `Person` constructor, I will get the same type of object back. 

```
// define Person constructor 
function Person(name) {
  
  // use the setName method to apply name value to the object
  this.setName(name);
}

// create prototype printName() method
Person.prototype.printName = function() {
  
  // console.log the value of name currently written to the person object
  console.log(this.name);

  return this;
};

// create prototype setName() method so you can
// set another name to the Person object as not to 
// create another instance of Person
Person.prototype.setName = function(name) {
  this.name = name;

  return this;
};

const person = new Person("Jill");
person.printName().setName("Tom").printName();
```

### But what about ES6?

ES6 updates the constructor and prototype method by introducing classes. 

```
class Person {
  constructor(name) {
    this.setName(name);
  }

  printName() {
    console.log(this.name);

    return this;
  }

  setName(name) {
    this.name = name;

    return this;
  }
}

const person = new Person("Jill");
person.printName().setName("Tom").printName();
```

### Pre-fab methods 

Any example of a pre-fab method is the `filter()` method. This is useful in functional programming as it reads clearly that you are going to **filter** on data. 

In this example, I am going to create an array of numbers and `filter()` on the `odd` numbers. 

```
// create new function; return boolean based on integer in the array 
function getOddNumbers(i) {
  return i % 2 !== 0;
} 

// set array
let myArray = [1,2,3,4,5,6,7];

// set variable using the array, filter() method 
// and passing in getOddNumbers function
const filteredArray = myArray.filter(getOddNumbers)

// console out new filtered array
console.log(filteredArray);
```

## Working with numbers
* `parseInt()` is a key function for converting strings to integers, but strips out the floating point value 
* `parseFloat()` returns the integer with the floating point value
* `var myObject =  Math.round(parseFloat('13.9 fo'));` gets floating point value and rounds up 
	* `var myObject =  Math.floor(parseFloat('13.9 fo'));` will return `13`

## Random numbers
* Let's simulate rolling dice. Need a return from 1-6.
	* `var myObject =  Math.ceil(Math.random() * 6);`

## Capture phase or bubble phase
Basically the difference of where the function starts. Think `trickle down` versus `bubble up`

You can test this using the `.bubbles` property. 

```
var x = event.bubbles; // returns true
```

## Conditional statements
Structure of an `if()` statement is 

```
if ([condition]) {
 // response 
}
```

There is no `;` after a `if()` statement. 

```
var answer = "ruby";
answer = answer.charAt(0).toUpperCase() + answer.slice(1);

if (answer === "Ruby") {
  document.write('YAY');
} else {
  document.write('BOO!');
}
```

Use of `&&` (and) or `||` (or) operators. 

```
var age = 25;

if (20 < age && age > 30) {
  comsole.log('correct');
} else {
  console.log('false');
}

var poolOpen = true;
var tempWarm = false;

if (poolOpen || tempWarm) {
	console.log ('let\'s go swimming!');
}
```

## Coercion

```
9 === 9 is TRUE
"9" == 9 is TRUE // the string of "9" can be coerced into being an integer 
"9" === 9 is FALSE // the string is not strictly equal to the integer 

'' == 0 is TRUE // that's just bad code!
```


## Common Functions with arguments 

### Common syntax for function declarations

```
function myNewFunction() {
 ...
}

function _myNewUnderscoreFunction() {
 ...
}

function $myNewjQueryFunction() {
 ...
}

```

Running example

```
function alertRandom() {
  const sidedDice = prompt('How many sides to the die?');
  
  const randomNumber = Math.ceil(Math.random() * sidedDice);
  console.log(randomNumber);
}

alertRandom();
```

### Common syntax for anonymous function expressions

```
// ES5; notice the function() expression
var randomNumberEs5 = function(dice) {
  var randomNumber = Math.ceil(Math.random() * dice);
  console.log(randomNumber);
};

// ES6; notice the function argument in text between the = and => (fat arrow syntax)
const randomNumberEs6 = dice => {
  const randomNumber = Math.ceil(Math.random() * dice);
  console.log(randomNumber);
};
```

A self executing function 

```
(function randomNumber(dice) {
  const randomNumber = Math.ceil(Math.random() * dice);
  console.log(randomNumber)
})(6);
```

What are the differences between these types of functions? Why is one better to use than another? 

The answer comes down to definition and scope. In ES6 anonymous function will be named by it's var value. So, it's not exactly anonymous anymore and will be named in the console if there is an issue. 

The next difference is the reference scope of `this`. 

Common function with a `return` statement versus executing output

```
function addNumbers(a, b) {
  var numbers = a + b;
  return numbers;
}

console.log(addNumbers(1, 20));
```

### Three ways to reverse a string
[review article](https://medium.freecodecamp.com/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb)


## Nested functions with arguments

Build a nested function that takes `3` independent arguments to come up with a total value.

```
function sum(arg0) {
  return arg0;
}

console.log(sum(10));  // returns 10
```
Next, add in nested function for second argument. The return of the nested function should add the two arguments and then the previous return should reference the nested function. 

```
function sum(arg0) {
  function addValue(arg1) {
    return arg0 + arg1;
  } 
  return addValue;
}

console.log(sum(10)(1)); // returns 11
```

Rinse and repeat. 

```
function sum(arg0) {
  function addValue(arg1) {
    function addMoreValue(arg2) {
      return arg0 + arg1 + arg2;
    }
    return addMoreValue;
  } 
  return addValue;
}

console.log(sum(10)(1)(1));
```

## Function wth Click Event

Create a function that adds CSS to a button on a click event

```
// declare the function 
function actionButton() {
  // set the variable to find the dom element 
  const button = document.getElementById('action-button');
  
  // create the click event
  button.onclick = function() {
    button.setAttribute("style", "margin-left: 100px");
  };
}

actionButton();
```

Alternative way using `addEventListner` versus `onClick` event with alternate CSS solution 

```
function actionButton() {
  const button = document.getElementById('action-button');
  
  button.addEventListener("click", function() {
    button.style.marginTop = "100px";
  }, false);
}

actionButton();
```

## Typical FOR loop

First create something to loop through; in this example I took a string, split it up and reversed it. Then using that array, I got it's length for the loop. 

Syntax for loop is three parts; set value for `i`, set the loop `while i is less then array length` and then set iterator `i++`

```
for (let i = 0; i < [array length]; i++) {
  ...
}
```
Working example of code

```
const myString = "Hello world how are you today";
const myArray = myString.split(" ").reverse();
const myArrayLength = myArray.length;

for (let i = 0; i < myArrayLength; i++ ) {
  console.log(myArray[i]);
}
```

## A function that tests if there are dupes in a string

```
function hasDupes(string) {
  var hasDuplicates = (/([a-zA-Z]).*?\1/).test(string);
  console.log(hasDuplicates);
}

hasDupes('dale sande')
```

## What is a Constructor?
Basically there are two different types of functions. Standard functions that execute repeated tasks and then a constructor will create a new instance of the function. 

Convention difference is that a standard function is lowercase and a constructor is uppercase. 

```
function Book() {
  ...
}

var myBook = new Book();
```

Basically, a constructor is a `cooke cutter` object maker. 

## What is a closure 
I NEED TO UNDERSTAND THIS


## How to reverse a string; palindrome test

```
function isPalindrome(string) {
  string = string.toLowerCase();
  var reversed = string.split('').reverse().join('').toLowerCase();
  var success = true;
  
  if (string != reversed) {
    success = false;
  }
  
  console.log(success);
}

isPalindrome('Bob');
```


## ES6 ... things learned

### strict
At the head of every .js file you should use 

```
'use strict';
```

This will keep you from making common code mistakes. 

### let and const
In the case of `let` is all has to do with scope. See this example about `block level scoping`

```
function foo(arg) {
  let name = arg;
    
  (function bar() {
    // if you remove the let keyword, 
    // the value of name would be over-written to "bob"
    let name = 'bob';
    
    console.log(name);
  }());
  
  console.log(name);
}

foo('dale');
// expected response is "bob" "dale"
```

For const, this is different as JS will only allow this to be assigned once in any given scope. 

```
(function() {
	const student = 'James';
	
	function createStudent(name) {
	  const student = name;
	  return student;
	}
	
	console.log(createStudent('Dale'))
	console.log(student);
})();
```

*Rules:* Use `let` when you to reassign a variable's value or need to scope a variable at the block level. 

Use `const` when you don't want a variable's value to ever change within the same scope.

### ES6 and string templates 
ES6 allows for string templating over standard string concatenation. 

```
'use strict';

const data = { 
  favMovie: 'Star Wars', 
  watchedTimes: 340 
};

// build string with ES6 templates
let madLibs = `My favorite movie is ${data.favMovie} and I have seen it ${data.watchedTimes} times!`;

// ES5 build literal string with concatenation.
let newString = 'My favorite movie is ' + data.favMovie + ' and I have seen it ' + data.watchedTimes + ' times!';

console.log(newString);
console.log(madLibs);
```

### ES6 => functions
ES6 arrow functions, or Lambda functions, is a new syntax for the more common anonymous functions from ES5. For example:

```
// ES5; notice the function() expression
var randomNumberEs5 = function(dice) {
  var randomNumber = Math.ceil(Math.random() * dice);
  console.log(randomNumber);
};

// ES6; notice the function argument in text between the = and => (fat arrow syntax)
const randomNumberEs6 = dice => {
  const randomNumber = Math.ceil(Math.random() * dice);
  console.log(randomNumber);
};
```

A lambda without a perimeter would simply use the `()` parens 

```
// ES6; notice the function argument in text between the = and => (fat arrow syntax)
const randomNumberEs6 = () => {
  const randomNumber = Math.ceil(Math.random() * 6);
  console.log(randomNumber);
};
```

There is more power in regards to assigning the values of `this`, but ... 


### Default paramaters
ES6 has support for setting default values for arguments in the function. This is no Sass 2010. Anyway ... some ES5 shizz

```
function greet(name, timeOfDay) {
  name = name || 'Dale';
  timeOfDay = timeOfDay || 'morning';
  console.log(`Good ${timeOfDay}, ${name}!`);
}

greet();
```

Now, ES6 

```
function greet(name = 'Dale', timeOfDay = 'morning') {
  console.log(`Good ${timeOfDay}, ${name}!`);
}

greet();
```

### Rest operator 
The example speaks for itself

```
function newFunction(name, ...arg) {
  let arrayString = name + ', ' + arg.join(' ');
  console.log(arrayString);
}

newFunction('dale', 'this', 'is', 'Sass');
```

### Spread operator
Allows for any array to be merged into another array using the `...` syntax. 

```
const ogflavs = ['bean', 'coco'];
const newFlavs = ['orange', 'cherry'];
const inventory = [...ogflavs, ...newFlavs];

console.log(inventory);
```

This is also useful for passing an array into the arguments of a function. This gets pretty close to how I can pass a list into a mixin in Sass. 

```
function myFunction(name, school) {
	const myString = `My name is ${name}, and I went to ${school} when I was a kid.`
	console.log(myString);
}

let args = ['Dale', 'Madison High School'];

myFunction(...args);
```

### Destructuring 
Oh yeah, I know this ... 




















# JS tests 

## for loop inside a function 

The following is proving to be a classic challenge and the answer is closure.

Using the following example, how can you update this so that the function will output an incremented value to the console?

```
function createTimeoutLoop() {
  let i = 0;
  
  for (i = 0; i < 3 ; i++) {
    setTimeout(function() {    
      console.log('The value is: ' + i);
    }, 500)
  };
}

createTimeoutLoop();
```

The answer I think best fits this question is to return the value of the for loop as a closure.  

```
function createTimeoutLoop() {
  
  // establish the for loop
  for (let i = 0; i < 3; i++) {
    
    // In the setTimeout function and pass in 
    // the value of i to the argument if e
    setTimeout(function(e) { 
      
      // create a closure using e as the iterator
      return function() { 
        console.log('The new value is: ' + e); 
      };
    
    // return i and then the time increment 
    // Each loop return requires the value of (500 * i)
    // or all the values will be returned at the same time
    }(i), 500 * i);
  }
}

createTimeoutLoop();
```

Further research, I like this answer better. More functional. 

```
let myOutput = (i) => {
  return () => {
    console.log(i)
  }
}

for (let i = 0; i <= 5; i++) {
  setTimeout(myOutput
    (i), 1000 * i
  )
}
```

[see link](http://brackets.clementng.me/post/24150213014/example-of-a-javascript-closure-settimeout-inside)

## Is this the question? 

Take in a string, split by letters, get it's length to do a loop, take each letter of the new array and push that into the empty array inside the loop, using that new array join it back together again into a single string and append to another string in the output. 

```
function welcome(name) {
  const fullName = [];
  const array = name.split('');
  const arrayLength = array.length;
  
  for (i = 0; i < arrayLength; i++) {
    fullName.push(name[i]);
  }
  
  const joinString = fullName.join('');
  const upperCaseString = joinString.charAt(0).toUpperCase() + joinString.slice(1);
  
  console.log("Welcome To HireOn " + upperCaseString);
}

welcome('dale');
```

## Find all [elements] and change color to BLUE

####Challenge from IMDb interview

Build a JS function that will find all the `anchor` tags (could be any element really) and change their color to blue. 

Totally screwed that one up. Here is the answer. It was a damned LOOP question! 

You need to get all the anchor tags in an array, then you need to loop through the array and individually append the color style on the iterator. 

```
colorLinks("blue");

function colorLinks(hex)
{
    var links = document.getElementsByTagName("a");
    for(var i=0; i < links.length; i++) {
      links[i].style.color = hex;
    }  
}
```

<!--## Iterate through all the buttons and add click event

Say for example you have a UI where there are multiple buttons with the same class and you need to add a click event on each button. 

The HTML

```
<button class="icon__button">
    <svg class="icon">
        <use xlink:href="#icon-thumbs-down"></use>
    </svg>
</button>
<button class="icon__button">
    <svg class="icon">
        <use xlink:href="#icon-thumbs-up"></use>
    </svg>
</button>
```

The JavaScript

```
// find all the buttons with this class
var buttonRef = document.querySelectorAll('.icon__button');

// loop through array and add the event of 'click' that will toggle a CSS class
for (var i = 0; i < buttonRef.length; i++) {
  buttonRef[i].addEventListener('click', function() {
    this.classList.toggle('icon__is-selected');
  });
}
```-->

## Create a string repeater
The idea here is to create a simple function that will take a string and an integer, the goal is to repeat the string x times in the console. 

```
function repeat(str, times) {
   let myArray = [];

   for (var i = 0; i < times; i++) {
     myArray.push(str);
   }
  
   myString = myArray.join(' ');

   return myString;
};

console.log(repeat('hello', 10));`
```

This is an advanced example where the function is actually appended as a native method to the `string` prototype.

```
String.prototype. repeat = function(times) {
   var str = '';

   for (var i = 0; i < times; i++) {
      str += this;
   }

   return str;
};

console.log('string '.repeat(5));
```

## Hoisting! 
What is the return of this function? 

```
function test() {
   console.log(a);
   console.log(foo());
   
   var a = 1;
   function foo() {
      return 2;
   }
}

test();
```

The result of this code is `undefined` and `2`. 

Both variables and functions are hoisted (moved at the top of the function) but variables don’t retain any assigned value. So, at the time the variable `a` is printed, it exists in the function (it’s declared) but it’s still undefined. 

So, if you are not able to assign a value until later in the function, set an empty var in the top of the function. This will then at least declare the variable and then reassign it's value later in the function. 

```
function test() {
   var a;
   function foo() {
      return 2;
   }

   console.log(a);
   console.log(foo());
   
   a = 1;
}

test();
```

## What is this scope of THIS
This interesting ...

```
var fullname = 'John Doe';

var obj = {
   fullname: 'Colin Ihrig',
   prop: {
      fullname: 'Aurelio De Rosa',
      getFullname: function() {
         return this.fullname;
      }
   }
};

console.log(obj.prop.getFullname());

var test = obj.prop.getFullname;
console.log(test());
```

Answer 

```
"Aurelio De Rosa"
"John Doe"
```

Why? `console.log(obj.prop.getFullname());` is executing the function within the scope of `var obj.prop`. So `this` is referring to the `prop` key. 

If you were to update to the following: 

```
var obj = {
   fullname: 'Colin Ihrig',
   getFullname: function() {
     return this.fullname;
   },
   prop: {
      fullname: 'Aurelio De Rosa',
      getFullname: function() {
         return this.fullname;
      }
   }
};

console.log(obj.prop.getFullname());
console.log(obj.getFullname());
```

then you would get the following: 

```
"Aurelio De Rosa"
"Colin Ihrig"
```

Makes sense. 

But ... what?

```
var test = obj.prop.getFullname;
console.log(test());
```

This outputs `"John Doe"`, why? This is because the var `test` is created in the global scope and when it execute the function from within `obj.prop.` the scope of `this` is global. Thus `"John Doe"`

Remove `var fullname = 'John Doe';` and you get `undefined`.

### Mind bender, call() and apply()

Add the following code to the preceding example

```
console.log(test.call(obj.prop));
```

This will output `"Aurelio De Rosa"`, but why? `test` is in the global scope? The `call()` and the `apply()` methods force the scope of a statement. 

The difference is, the `call()` method requires that arguments are specified separately; the `apply()` method takes them as an array.

In this code example, the string `"Aurelio De Rosa"` fits both criteria. 








## Shitty Uber interview from a fucking GO developer 
var i;
var fib = []; // Initialize array!

fib[0] = 0;
fib[1] = 1;
for(i=2; i<=10; i++)
{
    fib[i] = fib[i-2] + fib[i-1];
    console.log(fib[i]);
}
