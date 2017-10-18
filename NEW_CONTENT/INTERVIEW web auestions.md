# General Web dev Qs- 
  
* Scenario: I am a new developer, and I read that if you take the CSS in the project and stick it into the head of the document, that will cut out a network request, please explain what are the tradeoffs of doing this and why it isn’t typically done.

```
I am assuming that they are referring to placing all the CSS in a <style> tag versus a <link> tag. 

The tradeoff is maintenance. It's typically more difficult to manage all the CSS for a site from page to page using this method. 

Although there are techniques that make this easier like Gulp and inline-source processing, and modular component development. 
```
  
* What is the difference between cookie storage and local storage in the browser 

```
Cookies only allow for retaining small amounts of string data and are referenced on the HTTP request and are typically read by the server. Although they can be read by the client. Cookies also have expiration dates. 

localstrage is used by the client, can store way more data than a cookie and has no expiration date. localstorage can retain key/value pairs, but not whole objects. 

To use:

// see if function exists
if (typeof(Storage) !== "undefined") {
    localStorage.setItem("fullname", "Dale Sande");
} else {
    // Sorry! No Web Storage support..
}
```

* What does CORS stand for and what does it do? 

```
Cross Origin Reference Sharing

It is a protocol for allowing restricted resources to be requested outside the initial domain of the resource. Images, CSS, and web fonts are all easily accessible. 

JavaScript AJAX requests are restricted by default because of same-origin security policies. 
```
 
 
 
# NODE/CSS Qs- 
 
  
* CSS is a visual style language, the main concept is the cascading aspect of it. Can you tell me about selector specificity?  

```
Selector specificity is based on the selector type. The calculated value of a selector's specificity is based on 4 elements;

1. inline styles
2. IDs
3. classes, attribute selectors, and pseudo-classes
4. HTML elements and pseudo elements 

Least specific being the * selector == 0,0,0,0
Most specific is an inline style == 1,0,0,0

Most common is a class selector == 0,0,1,0
An over-qualified CSS selector on an element == 0,0,1,1
An over-qualified ID on an element == 0,1,0,1

https://specificity.keegan.st/
```

* CSS has a float, used to create layouts. Tell me about floats and how they work?

```
A float is a CSS property that specifies that an element should be placed on the left or right side of a container, allowing for other inline elements to wrap around it. 

The float takes the floated element out of the normal flow of the page and this can cause a lot of layout frustration of not cleared correctly. The floated element will automatically become a block element and the parent container of the floated element gets a calculated height of 0.

Most common trick to clear a float is to use a .clearfix selector hack

.clearfix {
  zoom: 1;
}
.clearfix:before, .clearfix:after {
  content: "\0020";
  display: block;
  height: 0;
  overflow: hidden;
}
.clearfix:after {
  clear: both;
}

Or some will set the parent container to float left with a 100% width and overflow auto, but this can create UI issues in many cases. 


```
 
* What is a closure?  You are explaining it to someone who isn’t brand new, explain it like I am new to the language, but I am an experienced programmer. 

```
A closure refers to the context in which a function was declared in and the vars is has access to. Example:

function foo(string) {
  const concatString = 'What\'s your name?' + ' ' + 'I am ... ' + string;
  const punc = '!';
  
  function fooBar() {
    console.log(concatString + punc);
  }
  
  return fooBar();
}

foo('bat-man');
```

* In the inner function, you have access to outer-scope, step me thru the process in the closure. Is there any sense of temporal aspect to the variables that the closures have?  

```
Within the closure, the function fooBar() will first look for the value of concatString within it's own scope, but then when not found it will look in it's parent's context. 
```

* What is an object literal in javascript?   

```
A list of key/value pairs contained within curly `{}` brackets. 

var cars = { 
  carTypes: {
    a: {
      make: 'Toyota',
      model: 'Sienna'
    },
    b: {
      make: 'Ford',
      model: 'Explorer'
    }
  }
};

console.log(`${cars.carTypes.a.make} ${cars.carTypes.a.model}`);
console.log(`${cars.carTypes.b.make} ${cars.carTypes.b.model}`);
```

* What is variable hoisting? 

```
Variable hoisting is what the JavaScript engine does in the background when it runs your code. Example: 

console.log(x); // -> undefined
var x = 5;
console.log(x); // -> 5
console.log(y); // -> error, no var declared 
```
 
Let’s say we are inside a function, and we declare 1st line and 2nd line functions, and you declare a few at the very end (about 80 lines of code), can you use the variables that haven’t been declared and if you can, why is that possible?  

```
If a variable had not been declared with the scope of the function, the engine will look to it's parent container looking for the declaration and value. Example: 

var y = 10;

function foo() {
  console.log(x); // -> undefined
  var x = 5;
  console.log(x); // -> 5
  console.log(y); // -> 10
}

foo();
```

* How do you use strict? 

```
'use strict';

You could also use it within the scope of a function

function foo() {
	'use strict'
	...
}
```

* Loosely typed and dynamically typed—define 

```
Loosely typed === user does not need to define the data type of the variable explicitly. The value of the variable is assessed at run time. 

Dynamically typed means that value types can change at run time 
myvar = 1; // number
myvar = “Toyota”; // string
myvar = ["Saab", "Volvo", "BMW"]; // array

Using strictly typed or manor of type checking prior to runtime will help keep errors from appearing in the compiled code. 

```
 
 
 
Javascript Qs- 
 
  
* You have a complied .js language, but typescript has no run time component, if the compiler misses something, the code that comes out, is it dynamically typed, loosely typed or not? 
 
```
it's still dynamically typed 
```
  
* Promises vs. call backs 
 
```
Callbacks are nested calls that depend on the callback to be completed before the next action can fire. The main complaint about them is the tendency to nest callbacks and end up with extremely complex code. 

Promises on the other hand, is just that. Request data, and once data is received then execute function. 

This is most commonly used with getting API URLs and data. The pro here is that the application will not get blocked from rendering what it can while it waits on a potentially slow response. 
```
 
* Performance Scenario: if you deploy a commit in node application and you see a performance regression, what are the tools you would use to debug and identify where the regression is happening? 

```
My first instinct would be to look in the Chrome Inspector Network tab and see if I can ID the issue. 
```
 
-chrome debugging tools are great! 
