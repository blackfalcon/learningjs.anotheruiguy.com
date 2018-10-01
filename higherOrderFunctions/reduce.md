> {{book.title}} v{{book.version}}

# Intro to the reduce() function

Introduced with the new ES6 spec, the `reduce()` method is the 'swiss army knife' of higher-order functions.

> The `reduce()` method applies a function against an accumulator and each element in the array (from left to right) to reduce it to a single value.

Up to this point, methods like `filter()` which will 'filter' out all the items of an array and produce a new simplified array have been discussed. Then `map()` which will iterate through the array and perform the function on each item to again produce a new array. Then `find()` which quickly finds and returns the first item in the array that matches a condition.

All these new tools greatly simplify the code, reduce the dependency on manually iterating over these large arrays and reduce opportunities for code bugs.

Now it's time to talk about the `reduce()` method. This method is a powerful tool for taking a large array and processing common values down into a new array. Not a filter or find, all the same data is there, it's just 'reduced'. The `reduce()` API is pretty simple. Just like other array methods, you append the `.reduce()` method onto the array itself.

Most of the confusion comes next where you need to enter your callback function and an optional `initialValue` argument.

```js
arr.reduce(callback[, initialValue])
```

The next step is how the callback function is constructed, what is commonly referred to as the **reducer**. The reducer itself requires two parameters, `accumulator` and `currentValue`, these arguments that the `reduce()` method will use to iterate over the array.

```js
(accumulator, currentValue) => expression
```

Imagine a self-feeding loop here. On the first pass of the `reduce()` method on the array, the `accumulator` will be empty or set to whatever you set as the `initialValue` value. Then the `currentValue` argument will hold the data for the next iteration. In the following code example, we will run code that will visually express these values.

## Introductory example

Imagine a simple scenario, one where there is an array of numbers and the need to return a sum of the array. A traditional way to solve this problem would be to run it through a `for()` loop, as illustrated in the following example.

Running this code, notice that with each loop over the array, the loop is taking the value of `total` and adding the value of `myArray[i]`

```js
const myArray = [1, 2, 3, 4, 5, 6]
let total = 0

for (let i = 0; i < myArray.length; i++) {
  console.log(`${total} + ${myArray[i]} = ${total + myArray[i]}`)
  total = total + myArray[i]
}

console.log(total)
```

Using the `reduce()` method on the array, there is no need for a `for()` loop or the manual tracking of the `total` value. In this example, the full process is illustrated to expose all the moving parts.

Starting out again with the same `myArray` of data, instead of having a `for()` loop and a global variable for `total`, what is shown is an example of a reducer function.

> Note: `reducer` is not a name convention or keyword. Call it `foo`, `bar` or whatever.

The `reducer()` function has two parameters per the method API, `(accumulator, currentValue)`. These names are not required, it's common to see `(accu, val)`.

The `console.log()` statement is here so that when this code is run, the iterations of the method will be visible.

This reducer function returns `accu + val`, with each pass inside the `reduce()` method the return value of `accu` and `val` will be updated.

The final part is running the `reduce()` method on the array. Per the API, the `callback` function is required and `initialValue` is optional. In this example, the `reduce()` method is using `reducer` as the callback and `0` as the `initialValue`.

```js
const myArray = [1, 2, 3, 4, 5, 6]

const reducer = (accu, val) => {
  console.log(`${accu} + ${val} = ${accu + val}`)
  return accu + val
}

const total = myArray.reduce(reducer, 0)

console.log(total) // 21
```

Running this code, the following would be logged to the console. `0` is the `initialValue` that is used for the first `accu` in the loop. If not there, it would start with the first number of the array.

```js
"0 + 1 = 1"
"1 + 2 = 3"
"3 + 3 = 6"
"6 + 4 = 10"
"10 + 5 = 15"
"15 + 6 = 21"
21
```

Since most of the code in our reducer function so far is only for illustration purposes, this can easily be distilled down to a one-line expression.

```js
const myArray = [1, 2, 3, 4, 5, 6]

const total = myArray.reduce((accu, val) => accu + val, 0)

console.log(total) // 21
```
