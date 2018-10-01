> {{book.title}} v{{book.version}}

# Reduce an array of objects

Let's imagine building an app to track purchases in a cart. With each item added to the cart, it will add a new item object to a purchase array. The following is an example of how this data may look.

```js
const purchase = [
  {
    item: 'Knight Rider Remote Car',
    amount: 56.99
  }, {
    item: '18inch Hulk Plush Toy',
    amount: 18.99
  }, {
    item: 'X-Wing Lego Set',
    amount: 118.99
  }
]
```

The task, take this data and quickly return a cart total. A solution _could_ run this through a `for()` loop and return the answer. But that is pretty procedural and prone to error.

```js
let purchaseTotal = 0;

for (let i = 0; i < purchase.length; i++) {
  purchaseTotal += purchase[i].amount
}

console.log(purchaseTotal)
```

Prior to looking at the actual solution, here is a 'long hand' version of the code to illustrate how this will work.

There are two parts to this code, the callback function, `callback()`, and then the `purchaseTotal` variable. Its value is created by using the `reduce()` method on the `purchase` array. The `reduce()` method uses the `callback()` function, and its `initalValue` variable is set to `0`.

The first line in the `callback()` function is a `console.log`, that when run, will log the values that are looped over and reduced.

```js
const callback = (accumulator, currentValue) => {
  console.log(accumulator, currentValue.amount)
  return accumulator + currentValue.amount
}

const purchaseTotal = purchase.reduce(callback, 0)
```

Running this code will output the values illustrated below, but this example has been commented to ensure clarity. Notice how the `reduce()` method feeds itself with the return of the method's value over each iteration that gets assigned to the `accumulator`. Then using the `accumulator` value plus the next iteration's value, `currentValue.amount`, the function will get the next return, and so on.

```js
accumulator         = 0
currentValue.amount = 56.99 + 0
accumulator         = 56.99
currentValue.amount = 18.99 + 56.99
accumulator         = 75.98
currentValue.amount = 118.99 + 75.98
accumulator         = 194.97
```

On the final loop of the method, the value of the `accumulator` is returned as the value of `purchaseTotal`.

The following example removes the `console.log` statement and leaves only the code necessary to run this functionality.

```js
const callback = (accumulator, currentValue) => {
  return accumulator + currentValue.amount
}

const purchaseTotal = purchase.reduce(callback, 0)
console.log(purchaseTotal) // 194.97
```

This works, but using the arrow syntax can reduce the code needed for this function.

```js
const callback = (accumulator, currentValue) => accumulator + currentValue.amount
const purchaseTotal = purchase.reduce(callback, 0)
console.log(purchaseTotal) // 194.97
```

The following example, the syntax is reduced even further to put the arrow function inline.

```js
const purchaseTotal = purchase.reduce((accu, val) => accu + val.amount, 0)
console.log(purchaseTotal) // 194.97
```

Purely functional. No issues with scope variables. Less opportunity for bugs.

## reduce(), filter() and map()

Having done this work, and having learned some basics about `filter()` and `map()`, one can easily see how the functionality be expanded in this example. What is needed next is a way to filter out the highest value in the array of objects and then map that to its item name and return that as a value.

The following example illustrates how to find the highest value in the array using a lot of imperative code, something many developers may be more familiar with.

```js
const highPrice = (el) => {
  let value = 0

  for (let i = 0; i < el.length; i++) {
    if (el[i].amount > value) {
      value = el[i].amount
    }
  }

  return value
}
```

The following reduce function can now be used in the following filter/map function and return the value required.

```js
console.log(purchase.filter(el => el.amount == highPrice(purchase)).map(el => el.item)) // ["X-Wing Lego Set"]
```

While this works, it is pretty procedural and can be done better. In this example, applying the `reduce()` method on the `purchase` array, the function within is using a terse `if()` syntax to evaluate the highest value in the array. This is much like the previous function, but eliminates the `for()` loop.

```js
const highPrice = purchase.reduce((acu, val) => (acu.amount > val.amount) ? acu.amount : val.amount)
```

As an alternate example, using the `Math` object, its `max` method and then chaining on the `apply()` method, a result can be returned in one line. The `apply()` method takes two arguments, `thisArg` which we will replace with the `Math` object, and then the `argsArray` which we will create on the fly using the `map()` method.

With these tools at our ready, we can replace a whole function to a single line expression.

```js
const highPrice = Math.max.apply(Math, purchase.map(el => el.amount))
```

The final step in this solution is to run the `filter()` method on the `purchase` array followed by the `map()` method to return the highest priced item in the array.

```js
console.log(purchase.filter(el => el.amount == highPrice).map(el => el.item)) // ["X-Wing Lego Set"]
```

These last two lines simply read better. It's expressive and one can quickly understand without having to read through lines and lines of other code to understand its result.

## In conclusion

Here is the code that would be considered a good solution to the problem.

```js
const purchase = [
  {
    item: 'Knight Rider Remote Car',
    amount: 56.99
  }, {
    item: '18inch Hulk Plush Toy',
    amount: 18.99
  }, {
    item: 'X-Wing Lego Set',
    amount: 118.99
  }
]

const purchaseTotal = purchase.reduce((accu, val) => accu + val.amount, 0)
console.log(purchaseTotal) // 194.97
const highPrice = purchase.reduce((acu, val) => (acu.amount > val.amount) ? acu.amount : val.amount)
console.log(highPrice) // 118.99
console.log(purchase.filter(el => el.amount == highPrice).map(el => el.item)) // ["X-Wing Lego Set"]
```
