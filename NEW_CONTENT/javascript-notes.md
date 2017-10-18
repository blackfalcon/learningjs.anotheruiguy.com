# What I learned about JavaScript 101

## A minute on scoping

You, reading this, may have had some experience with JavaScript and may have had discussion with other developers about scoping. Good. These are great concepts. Just be aware that the initial examples listed here are NOT SCOPED. This is intended so that we can get through concepts quickly and without having to trigger events. The scripts that initially follow will contain global variables and have methods that fire immediately when loaded into the DOM. When doing responsible programming, you won't write code this way, but hey, we are learning here, right?

## Objects, events and methods

1. The use of object is an __instance__
1. Objets have
	1. Properties (characteristics or keys)
		1. Properties/keys can be shared between objects
		1. Objects that share properties may have different values
		1. Properties are described in key:value pairs
	1. Events (the point in which someone interacts with the computer)
	1. Methods

Example of a JavaScript object type for a hotel:

```
hotel {
	name: 'Quay',
	rating: 4,
	rooms: 42,
	bookings: 21,
	gym: false,
	pool: true
}
```

Example of a car object type that share properties/keys but have different values:

```
car {
	make: 'BMW',
	currentSpeed: '30pmh',
	color: 'silver',
	fuel: 'diesel'
}

car {
	make: 'Porsche',
	currentSpeed: '20mph',
	color: 'silver',
	fuel: 'gasoline' 
}
```

Events are the things that happen when a person interacts with an object in the program. An example of an event with a car type object would be:

```
event {
	brake: 'car slows down',
	accelerator: 'car speeds up'
}
```

Methods are the things that need to happen when interacting with an object. In these examples, a method would be `changeSpeed()` and it would evaluate the `currentSpeed` of an object and based on the event of `brake` or `accelerator` it would speed up or slow down. 

## The web browser

1. The presence of a page is the __Document Object__
1. The browser uses all the HTML elements and converts them to __nodes__ to create the __Document Object Model__
1. The DOM and the nodes within are stored in a key:value pair

An example of a DOM tree structure would be the following:

```
webpage {
	html: {
		head: {
			title: 'Bob's Construction',
			link: 'css/style.css'
		},
		body: {
			h1: 'Welcome to Bob's',
			p: 'We are #1 in construction, bar none!'
		}
	},
}
```

## An example

For the __Javascript__ We don't care about presentation right now, or what this is doing. What's interesting to note is that in this example, the variable we declare is treated as an object as we perform a method to get a value. 

```
var today = new Date();
var hourNow = today.getHours();
var greeting;

if (hourNow > 18) {
	greeting = 'Good evening!' + " It's " + hourNow + ' hundred hours';
} else if (hourNow > 12) {
	greeting = 'Good afternoon!' + " It's " + hourNow + ' hundred hours';
} else if (hour > 0) {
	greeting = 'Good morning' + " It's " + hourNow + ' hundred hours';
} else {
	greeting = 'Hi there!' + " It's " + hourNow + ' hundred hours';
}

document.write('<h3>' + greeting + '</h3>');
```

Then for the HTML we could include the JavaScript directly into the view like the following: 

```
<!DOCTYPE html>
<html>
	<head>
		<title>This is a web page</title>
		<link rel="stylesheet" href="style.css" />
	</head>
	<body>
		<h1>Bob's Construction Palace</h1>
		<script src="add-content.js"></script>
		<p>these are more words</p>
	</body>
</html>
```

## Let's take that apart a little

`document` is the object in this example

`.` or DOT is the member operator

`write()` is the method being performed on the document object.

What's between the parens `()` are the parameters that can be passed in. We could have easily just have said `document.write('Good Morning!`); and that would have worked as well, but that would be less awesome.

### Javascript RUNS where it is called in

Important to remember, where the Javascript statement is called into the DOM is exactly where the Browser will run that code. 

This is important as the browser stops what it is doing, reads the script and then checks to see if it needs to do anything. This is called a __blocking event__.

### Decompose some JavaScript

Each line of code that contains an instruction is called a __statement__ and ends with a semi-colon `;`.

```
greeting = 'Good evening!' + " It's " + hourNow + ' hundred hours';
```

Between the curly braces `{}` is called a __code block__.

`if` and `else` statements are directives that instruct the code what to do.

### Variables

Variables are important because they store information into an object as a string, number or boolean value. Storing data into a variable is also a way to increase speed and performance of a script. For example:

```
var foo = 10 * 100;
```

We can take `foo` and call that into a number of operations in our code and it always equals `1000`. This is just faster. If we always do the calculation in the point of the script we need it, then the processor needs to perform that calculation again and again. 

### Naming conventions

When a JavaScript variable uses more then one word to describe it, you are to use camelCase. Example is `fooBar`. 

### Variable declaration 

When declaring a new variable, you should use the `var` keyword. The scope of a variable declared with `var` is its current execution context. Otherwise it would be considered a global variable and that can have unintended consequences. 

Once a variable is declared, you can assign it a value using the __variable name_ and __assignment operator__  and the the __variable value__.  Example is:

```
var fooBar = 'this is fubar!';
```

In this example I used a __string data type__, there is also the __number data type__ `0 - 9` and then there is the __boolean data type__ which always resolves to `true` or `false`.  


## Use a var to store a number and return a value to the DOM

Using a variable we can store numbers, use those values to perform an operation to crate another value to a variable and then using JavaScript methods we can produce a string that is used as content in the DOM. First out JavaScrip.

```
// Create three variables to store the information needed.
var price;
var qty;
var total;

// Assign values to the price and quantity variables.
price = 5;
qty = 14;
// Calculate the total by multiplying the price by quantity.
total = price * qty;

// Get the element with an id of cost.
var el = document.getElementById('cost');
el.textContent = '$' + total;
```

For the most part, this is pretty straight forward. Declare some variable names, assign values to the vars. For the `total` var we operate on `price` and `qty` to get a value. It's the next part that get's interesting. 

It's common to see `el` as a variable. It's a common JS shorthand for `element`. In this statement we are setting the value of `el` at the time we are declaring the variable. We are looking for the `document` object and using the `getElementById()` method and passing in the param of `'cost'`. This will tell JavaScript to search the DOM looking for the node with the ID of `cost` on it. 

In the next statement we tell the `el` variable to use `textContent` property. The __textContent__ property represents the text content of a node and its descendants. So in this statement we are saying, "Go get the element with the ID of `cost` and then set the text content to ..." And in this case we are saying, add in the string of `$` + the value of `total`. 

Now remember, placement of the script counts. If we were to call in this script BEFORE the browser was able to read the HTML and create the DOM tree, then it has no way of knowing the placement of `cost` and it's content node within and you would probably see an error of `Uncaught TypeError: Cannot set property 'textContent' of null` in the console. 

```
<!DOCTYPE html>
<html>
  <body>
    <h1>header</h1>
    <div id="cost">Replace me with the real cost!</div>
    <script src="exmaple.js"></script>
  </body>
</html>
```














