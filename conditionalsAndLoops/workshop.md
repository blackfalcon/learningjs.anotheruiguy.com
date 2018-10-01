> {{book.title}} v{{book.version}}

# Workshop

<!--
This is an example document for the purpose of standardizing workshop content.

Please create bullet points that will be the questions asked of the students, then follow that with an expected outcome. These answers are not shared with the students, but are there to ensure that whomever is leading the class with pre-written content understands the intent of the author.
-->

This Confidence Course Workshop will challenge you to write code that fits the following spec.

* Using a `if()` conditional statement, evaluate if `var a` is equal to `var b`. Display return in an `alert()` prompt.

```js
var a = 10;
var b = '10';

if (a === b) {
  alert('winning')
} else {
  alert('not equal')
}
```

* Refactor the previous `if()` statement to use a conditional (ternary) operator

```js
var a = 10;
var b = '10';

a === b ? alert('winning') : alert('not equal')
```

* Using a basic `for()` loop, create a loop that will print `0` - `9` to the console.

```js
for (let i = 0; i < 10; i++) {
  console.log(i)
}
```

* Using a while loop, iterate over the letters of a string and print each letter to the console

```js
let str = "JavaScript"
let i = 0;

while(i < str.length) {
  console.log(str[i])

  i += 1;
}
```

* Create a simple object with 3 or more properties, using a `for ... in()` loop, loop through the enumerable properties and print their values to the console.

```js
const obj = {
  color: 'red',
  size: 'large',
  material: 'metal'
}

for (let property in obj) {
  console.log(`My car is ${obj[property]}`)
}
```

* Given an array of items, use a `for ... of()` loop to iterate over the elements and print their values to the console.

```js
const items = ['red', 'large', 'metal'];

for (let item of items) {
  console.log(`My car is ${item}`)
}
```

* Given an array of elements, use a `forEach()` method on the array to print out the elements to the console.

```js
const arr = ['red', 'large', 'metal'];

arr.forEach(function(e) {
  console.log(`My car is ${e}`)
})
```
