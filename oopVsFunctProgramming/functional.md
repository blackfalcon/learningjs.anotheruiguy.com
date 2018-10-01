> {{book.title}} v{{book.version}}

# Functional(Declarative) programming

> A programming paradigm—a style of building the structure and elements of computer programs—that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data.

Functional programming, while on the surface, may appear to be very simplistic, it can be pretty tricky to make sure you are actually doing it correctly. One thing that usually gets thrown into simpler explanations is to use higher-order functions, like replacing `for()` loops with `.filter()` and `.map()`. But there is way more than that IMHO.

For starters, there is the _function first_ perspective. Simply meaning, that unlike OOP, functional programming has two very separate parts, the data AND the functions, whereas on OOP, the methods are typically bound to the data itself. This may seems simplistic at first, but it can be tricky, especially with the concept of `this` and issues of mutability.

When working with OOP in JavaScript it's pretty typical to see methods that will mutate the state of the data within the encapsulation of the object. In some cases that can bite you back, on other cases it may be okay. But when writing code using the Functional programming style, mutating the state of data is your #1 enemy.

In Functional programming the #1 law is, consistency of the functions, or what is called _pure functions_. This means is that whenever you use a function, the outcome is predictable.

What does this look like in code? A simple example is to say that we have a variable in the outer scope of the function and our `addOne` makes use of that value by adding one each time the function is run.

```js
var x = 0;

function addOne() {
  x += 1;
  console.log(x);
  return x;
}

addOne(); // 1
addOne(); // 2
addOne(); // 3
addOne(); // 4
addOne(); // 5
addOne(); // 6
```

This code reads pretty straightforward and it's clear that the `addOne()` function is using the outer scope variable and with each pass, it is over-writing the value of `x`, so now with each pass, you will get a different output. While in this example this is pretty clear when getting into production scale code, not having any idea what the state of the data going into the function and being unaware of what data coming out can cause you hours of frustration.

For argument sake, even if we try to encapsulate this in a function, we are still mutating the outer scoped variable. In the following example I am basically doing the same thing, but since I set `x = z;` I am mutating the state of `x` and that ruins everything.

```js
var x = 0;

function addOne(z) {
  z += 1;
  console.log(z);
  x = z;
  return x;
}

addOne(x); // 1
addOne(x); // 2
addOne(x); // 3
addOne(x); // 4
addOne(x); // 5
addOne(x); // 6
```

What does a pure function look like? Using the outer scope of `x`, this time the `addOne()` function adds `1` and then returns the value. That's it. It has no effect on the outer scope and each time you write that function calling on the same variable you get ... wait for it ... a predictable outcome!

```js
var x = 0;

function addOne(z) {
  z += 1;
  console.log(z);
  return z
};

addOne(x); // 1
addOne(x); // 1
addOne(x); // 1
addOne(x); // 1
addOne(x); // 1
```

What this is illustrating is that that a pure function will always take in a value as its starting point, perform an action on that value, output a new value that does not mutate the original value.

## Let's dig into some code

JavaScript allows us to easily manipulate the data of an object at any point in our code. While powerful and sometimes useful, this can also get us in a lot of trouble.

Let's imagine that we have a rare toy shop and the app we need to build will consist of a few objects that represent toys and then we will have a series of pure functions that we can use like methods on the objects and return data.

Keep in mind, we can create functions that mutate the data of the object for the purpose of the instance itself, but it will not mutate the state of the data for the next function that is applied, even if the same function is applied.

Let's make a couple toy objects.

```js
const EXP10087 = {
  character: 'Luke Skywalker',
  year: 1977,
  style: 'Telescoping saber',
  film: 'Star Wars; A New Hope',
  value: 650,
  condition: 'mint on card'
}

const EXP22571 = {
  character: 'Han Solo',
  year: 1977,
  style: 'Black vest',
  film: 'Star Wars; A New Hope',
  value: 150,
  condition: 'Lightly used'
}
```

The first function I want to create is one that will print back the contents of the object. Notice that I am not passing in an argument that will be the value of `this`. I am intentionally doing this so that the function will be extremely flexible in its use.

```js
function printObj() {
  return `${this.year} ${this.character} from ${this.film}. ${this.style}, ${this.condition} $${this.value}.`
}
```

To make use of this function, and others, I am going to use the `.call()` method as we will be passing in the object we need data from and any individual arguments needed.

```js
console.log(printObj.call(EXP10087));
// "1977 Luke Skywalker from Star Wars; A New Hope. Telescoping saber, mint on card $650."
```

The next feature, I need to be able to return a surge price. I need the ability to enter an integer into a function and return a percentage used to calculate the new surge price of the toy.

```js
function calcPercent(percentage) {
  return (percentage / 100);
}
```

Now no matter where we use this, we can simply pass in an integer and it will output a decimal value to the 100th place. A percentage. Next we build the `suregePrice()` function that will make use of-of our new `calcPercent()` function.

For our new surge price, I will use the percentage of increase and multiply that by the value of `this`. E.g., if the toy value is $100 and there is a 15% increase, the surge price will be an additional $15. Next, we add that to the current value of `this`. The last step is to log this new surge price statement to the console.

```js
function surgePrice(percentage) {
  const newValue = this.value + (this.value * calcPercent(percentage));
  return `${this.year} ${this.character}. ${this.condition} SURGE PRICE: $${newValue}.`
}
```

Note there is no mutation of the toy object. Using this function will alter the value of the toy for its purpose, but the object's value did not change and any subsequent function that comes after will not be affected by running this function.

To use, call the `surgePrice()` function, but like the `printObj()` function we have a disconnected reference to `this` so we need to use the `call()` method to reference the object's data. Last, pass in the value of percentage increase. Since this Luke Skywalker is pretty hot, we will increase the price by 25%.

```js
console.log(surgePrice.call(EXP10087, 25)); // "1977 Luke Skywalker. mint on card SURGE PRICE: $812.5."
```

Next, we need a sale function. This will work exactly the same way as the surge price but in the opposite direction. Again we will make use of the `calcPercent()` function, that is why it was a good idea to make this an independent utility function.

```js
function salePrice(percentage) {
  const newValue = this.value - (this.value * calcPercent(percentage))
  return `${this.year} ${this.character}. ${this.condition} SALE ${percentage}% off: $${newValue}.`
}
```

Exactly like the surge price, we will use this function with the `.call()` method, pass in the discount percentage and it will return a string to the console. Han isn't selling too good now, so we will have a 1/2 off sale.

```js
console.log(salePrice.call(EXP22571, 50)); // "1977 Han Solo. Lightly used SALE 50% off: $75."
```

## Conclusion

What we gain from this style of coding is that all these functions can be used on any object at any time and the output is consistent, predictable and testable. By avoiding the mutation of the object's data while in the scope of the running code, you maintain a single source of truth at all times. Now, you may want to have a function that would change the value of an object's data, but that would be a final step and that return would change the database and exit the function. Any further actions would pull new data from the endpoint.

```js
const EXP10087 = {
  character: 'Luke Skywalker',
  year: 1977,
  style: 'Telescoping saber',
  film: 'Star Wars; A New Hope',
  value: 650,
  condition: 'mint on card'
}

const EXP22571 = {
  character: 'Han Solo',
  year: 1977,
  style: 'Black vest',
  film: 'Star Wars; A New Hope',
  value: 150,
  condition: 'Lightly used'
}

function printObj() {
  return `${this.year} ${this.character} from ${this.film}. ${this.style}, ${this.condition} $${this.value}.`
}

function calcPercent(percentage) {
  return (percentage / 100);
}

function surgePrice(percentage) {
  const newValue = this.value + (this.value * calcPercent(percentage));
  return `${this.year} ${this.character}. ${this.condition} SURGE PRICE: $${newValue}.`
}

function salePrice(percentage) {
  const newValue = this.value - (this.value * calcPercent(percentage))
  return `${this.year} ${this.character}. ${this.condition} SALE ${percentage}% off: $${newValue}.`
}


console.log(printObj.call(EXP10087)); // "1977 Luke Skywalker from Star Wars; A New Hope. Telescoping saber, mint on card $650."
console.log(surgePrice.call(EXP10087, 25)); // "1977 Luke Skywalker. mint on card SURGE PRICE: $812.5."
console.log(printObj.call(EXP10087)); // "1977 Luke Skywalker from Star Wars; A New Hope. Telescoping saber, mint on card $650."
console.log(printObj.call(EXP22571)); // "1977 Han Solo from Star Wars; A New Hope. Black vest, Lightly used $150."
console.log(salePrice.call(EXP22571, 50)); // "1977 Han Solo. Lightly used SALE 50% off: $75."
console.log(printObj.call(EXP22571)); // "1977 Han Solo from Star Wars; A New Hope. Black vest, Lightly used $150."
```
