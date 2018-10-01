> {{book.title}} v{{book.version}}

# Making Decisions with Conditional Statements

When it comes to decision making in JavaScript, the `if()` statement is pretty clear and easy to work with. But in order to make use of conditional statements, you must first understand how comparison operators work. Then you need to understand the concept of truthy/falsy.

## Comparison operators

In JavaScript, comparison operators are the name of the game. So much logic is predicated on your understanding of these simple concepts.

Operator | Description | Examples returning true
--- | --- | ---
`==` | Returns `true` if the operands are equal. | `3 == '3'`
`!=` | Returns `true` if the operands are not equal. | `3 != "three"`
`===` | Returns `true` if the operands are equal and of the same type. | `3 === 3`
`!==` | Returns `true` if the operands are of the same type but not equal, or are of different type. | `3 !== "3"`
`>` | Returns `true` if the left operand is greater than the right operand. | `12 > 2`
`>=`| Returns `true` if the left operand is greater than or equal to the right operand. | `4 >= 3`
`<` | Returns `true` if the left operand is less than the right operand. | `2 < 12`
`<=` | Returns `true` if the left operand is less than or equal to the right operand. | `3 <= 4`

### Missing operators

You may notice that there are no default operators for returning `true` if the left operand is greater or less than and of the same type, e.g. `<==` or `>==`. For example, the following code illustrates that JavaScript will use type coercion whenever possible.

Here the number of `10` is less than the string of `'11'`, but the `'11'` type of string is dismissed and JavaScript sees this as a number and evaluates that `10` is less than or equal to `11`. Of course, this is not the answer we would want.

```js
var a = 10;
var b = '11';

var test = a <= b;

console.log(test) // true
```

To understand `a === b` you understand that this actually means `typeof a == typeof b && a == b` as illustrated in the following example.

```js
var a = 10;
var b = '10';

var test = typeof a == typeof b && a == b;

console.log(test) // false
```

Knowing that, we can evaluate if something is of the same type AND less than or greater than. In this example, `a` is NOT of the same type as `b`, thus failing the first part of the evaluation and returning false.

```js
var a = 10;
var b = '11';

var test = typeof a == typeof b && a <= b;

console.log(test) // false
```

## Conditional statements

The basic structure of a conditional statement is really to ask the question `if this, than what?` But it can even get more complicated than that. For example, `if this, or this and that, then what?` or what about `if this than that, but what else if this, than that?` While these questions may seem a little silly, these are real questions when writing a program.

For starters, the structure of an `if()` statement looks like the following.

```js
if ( ... ) {
  ...
}
```

That's it. Simply fill in the blanks and you are off and running with real conditional logic in code. For example, let's imagine that you are running a store and you need to be alerted when to re-order some products. You could have another program running that keeps count of the products on the shelf and it reports back an integer every so often. But when that number reaches a certain threshold, you are alerted to order more.

In the following example I have an array of products where there are nested arrays of the individual product and their quantity. In our `if()` statement I am asking the question, "_If there are exactly 15 or less than 15 milk bottles, alert me to order more._"

```js
const products = [['milk', 10], ['cheese', 18], ['bread', 14]];

if (products[0][1] <= 15) {
  console.log(`${products[0][0]} qty ${products[0][1]}: order more.`) // "milk qty 10: order more."
}
```

But there will be times when these conditions are not met. For example, let's imagine that none of the products are below the threshold for the order alert, what then? To wrap up an `if()` statement, you can add an `else` catchall. What this means is, if any condition is NOT meet, then do this.

```js
const products = [['milk', 16], ['cheese', 18], ['bread', 14]];

if (products[0][1] <= 15) {
  console.log(`${products[0][0]} qty ${products[0][1]}: order more.`)
} else {
  console.log('Shelves look great!')
}
```

To complicate things, let's imagine that we need to be alerted of two things before placing a new order. The `if()` statement can take what are called `&&` (and) operators. This statement says, "_This AND that thing need to happen_" for this to be true. For example ...

```js
const products = [['milk', 12], ['cheese', 18], ['bread', 14]];

if (products[0][1] <= 15 && products[1][1] <= 20) {
  console.log(`${products[0][0]} and ${products[1][0]} are running low, order more`) // "milk and cheese are running low, order more"
} else {
  console.log('Shelves look great!')
}
```

Along with `&&`, there is the `||` (or) statement. This is stating, "_This OR that thing need to happen_" for this to be true. But since we have alternate possibilities, we should have another `if()` statement. For this we can nest `if()` statements like so.

```js
const products = [['milk', 18], ['cheese', 3], ['bread', 14]];

if (products[0][1] <= 15 || products[1][1] <= 20) {
  if (products[0][1] <= 15) {
    console.log(`${products[0][0]} is running low, order more`)
  } else if (products[1][1] <= 20) {
    console.log(`${products[1][0]} is running low, order more`)
  }
} else {
  console.log('Shelves look great!')
}
```

When you are using integers like the previous examples illustrate, the code is pretty straight forward. But you can also use `true` or `false` statements to code your `if()` statements as well. For example, instead of putting the question inside the `if()` statement, we can pose this question to a variable and it will contain the `true` or `false` return.

In the example see how I moved the posed question from the `if()` statement to the variable of `milk`. Then in the `if()` statement itself I am only asking if `milk` returned true or not.

```js
const products = [['milk', 8], ['cheese', 3], ['bread', 14]];

let milk = products[0][1] <= 15;

if (milk === true) {
  console.log(`${products[0][0]} is running low, order more`) // "milk is running low, order more"
} else {
  console.log('Shelves look great!')
}
```

In this situation we can use a shortcut as well. We don't need to ask if `milk === true` we can simply ask for `milk` and that defaults to asking of the return is `true`.

```js
const products = [['milk', 8], ['cheese', 3], ['bread', 14]];

let milk = products[0][1] <= 15;

if (milk) {
  console.log(`${products[0][0]} is running low, order more`) // "milk is running low, order more"
} else {
  console.log('Shelves look great!')
}
```

Alternatively, we can use the `!` (bang) symbol to denote that the answer is `false`. See in the example below how the if() statement is asking `if (!milk) { }`.

```js
const products = [['milk', 28], ['cheese', 3], ['bread', 14]];

let milk = products[0][1] <= 15;

if (!milk) {
  console.log(`${products[0][0]} is well stocked!`) // "milk is well stocked!"
} else {
  console.log('WOAH! Milk is running low!')
}
```

## Conditional (ternary) Operator

The conditional (ternary) Operator is not new with ES6, but has been recently popularized with new emphasis on succinct code patterns. The syntax is surprisingly simple.

```js
condition ? expression1 : expression2
```

For the `condition`, simply pass in what you want to evaluate. This would be the same thing you put inside the `()` parens in an `if()` statement. E.g. `a === b` or `i < x`.

Next, `expression1` represents the return of `conditions` if true, leaving `expression2` to be the return if `condition` is false. See the following example for a standard `if()` statement with it's simplified ternary version.

```js
var a = 10;
var b = '10';

// if() statement
if (a === b) {
  alert('winning')
} else {
  alert('not equal')
}

// ternary
a === b ? alert('winning') : alert('not equal')
```

### if/else statements

With conditional (ternary) operators `if/else` statements are easily supported. Take the following example.

```js
var a = 10;
var b = '110';

if (a === b) {
  alert('winning')
} else if (typeof a == typeof b && a < b) {
  alert('so much winning')
} else {
  alert('so much fail')
}
```

Running this code the answer of course would be `so much fail`. To simplify this using the ternary pattern we can easily extend the syntax pattern.

```js
a === b ? alert('winning')
  : typeof a == typeof b && a < b ? alert('so much winning')
  : alert('so much fail')
```
