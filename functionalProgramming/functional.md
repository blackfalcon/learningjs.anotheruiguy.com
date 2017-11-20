# Functional programming

> A programming paradigm—a style of building the structure and elements of computer programs—that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data.

Functional programming, while on the surface, may appear to be very simplistic, it can be pretty tricky to make sure you are actually doing it correctly.

For starters, there is the _function first_ perspective. Simply meaning, that unlike OOP, functional programming has two very separate parts, the data AND the functions, whereas on OOP, the methods are typically bound to the data itself. This may seems simplistic at first, but it can be tricky at first, especially with the concept of `this` and issues of mutability.

When working with OOP in JavaScript it's pretty typical to see methods that will mutate the state of the data within the encapsulation of the object. In some cases that can bite you back, on most other cases, it's ok. But when writing using the Functional programming style, mutating the state of data is your #1 enemy.

In Functional programming the #1 law is consistency of the functions, or what is called _pure functions_. What this means is that whenever you use a function, the outcome is predictable.

What does this look like? A simple example is to say that we have a variable in the outer scope of the function and our `addOne` makes use of that value by adding one each time the function is run.

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

This reads pretty straight forward and it's clear that the `addOne()` function is using the outer scope variable and with each pass it is over-writing the value of `x`, so now with each pass you will get a different output. While in this example this is pretty clear, when getting into production scale code, not having any idea what the state of the data going into the function and being completely unaware of what the data is that will come out can cause you hours of frustration.

Just for argument sake, even if we try to encapsulate this in a function, but we are still mutating the outer scoped variable, we are still in trouble. In the following example I am basically doing the same thing, but since I set `x = z;` I am mutating the state of `x` and that ruins everything.

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

What does a pure function look like? Using the previous example, let's encapsulate. Still using the outer scope of `x`, this time the `addOne()` function adds `1` and then returns the value. That's it. It has no effect on the outer scope and each time you you write that function calling on the same variable you get ... wait for it ... a predictable outcome!

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

const EXP10778 = {
  character: 'Obe-Wan Kenobi',
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
  condition: 'lightly used'
}
```

The first function I want to create is one that will print back the contents of the object. Notice that I am not passing in an argument that will be the value of `this`. I am intentionally doing this so that the function will be extremely flexible in its use.

```js
function printObj() {
  return `${this.year} ${this.character} from ${this.film}. ${this.style}, ${this.condition} $${this.value}.`
}
```

To make use of this function, I need to use the `.call()` or `.apply()` method.

```js
console.log(printObj.call(EXP10087));
// "1977 Luke Skywalker from Star Wars; A New Hope. Telescoping saber, mint on card $650."
```

Great, we have a pure function for sure. For our next feature I need to be able to return a surge price because the toy is HOT!.

```js
function surgePrice(percentage) {
  const upValue = this.value * (percentage / 100)
  const newValue = this.value + upValue
  return `${this.year} ${this.character}. ${this.condition} SURGE PRICE: $${newValue}.`
}
```


```
const EXP10087 = {
  character: 'Luke Skywalker',
  year: 1977,
  style: 'Telescoping saber',
  film: 'Star Wars; A New Hope',
  value: 650,
  condition: 'mint on card'
}

const EXP10778 = {
  character: 'Obe-Wan Kenobi',
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
  condition: 'lightly used'
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

function discount(value, percentage) {
  const discountValue = value * calcPercent(percentage);
  return value - discountValue;
}

function multiSaleDiscount(objOne, objTwo, percentage = 10) {
  const totalValue = discount(objOne.value, percentage) + discount(objTwo.value, percentage);
  const str = `${percentage}% two toy discount, ${objOne.character} and ${objTwo.character} for $${totalValue}!`
  return str;
}


//console.log(printObj.call(EXP10087));
//console.log(salePrice.call(EXP10087, 50));
console.log(surgePrice.call(EXP10087, 50));
//console.log(printObj.call(EXP10087));

// console.log(multiSaleDiscount(EXP10087, EXP22571, 50));







```
