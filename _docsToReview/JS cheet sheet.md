# JS Basics

## Random numbers
* Let's simulate rolling dice. Need a return from 1-6.
	* `let myObject =  Math.ceil(Math.random() * 6);`

## Capture phase or bubble phase
Basically the difference of where the function starts. Think `trickle down` versus `bubble up`

You can test this using the `.bubbles` property. 

```
var x = event.bubbles; // returns true
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

### Anonymous function expressions

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

Common function with a `return` statement versus executing output

```
function addNumbers(a, b) {
  var numbers = a + b;
  return numbers;
}

console.log(addNumbers(1, 20));
```

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

## A function that tests if there are dupes in a string

```
function hasDupes(string) {
  var hasDuplicates = (/([a-zA-Z]).*?\1/).test(string);
  console.log(hasDuplicates);
}

hasDupes('dale sande')
```

## What is a Constructor?
Basically there are two different types of functions. 

Standard functions that execute repeated tasks and then a constructor will create a new instance of the function. 

Convention difference is that a standard function is lowercase and a constructor is uppercase. 

```
function Book() {
  ...
}

var myBook = new Book();
```

Basically, a constructor is a `cooke cutter` object maker. 

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
  name = arg;
    
  (function bar() {
    // if you remove the let keyword, the value of name would be over-written
    let name = 'bob';
    
    console.log(name);
  }());
  
  console.log(name);
}

foo('dale');
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

# JS tests 

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

## Iterate through all the buttons and add click event

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
```

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

console.log(repeat('hello', 10));
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

## Basic HTML structure 

```

<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <meta http-equiv="x-ua-compatible" content="ie=edge">
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>my title</title>

        <link rel="stylesheet" href="css/normalize.css">
        <link rel="stylesheet" href="css/main.css">
        
        <script src="js/vendor/modernizr-2.8.3.min.js"></script>
    </head>
    
    <body>
        <p>Hello world! This is HTML5 Boilerplate.</p>
        <script src="js/plugins.js"></script>
        <script src="js/main.js"></script>
    </body>
</html>
```
