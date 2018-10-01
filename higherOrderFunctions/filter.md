> {{book.title}} v{{book.version}}

# The filter() method

The ES6 `.filter()` array method does exactly what it implies.

> The `filter()` method creates a new array with all elements that pass the test implemented by the provided function.

The `filter()` function is a method on the Array object. It takes an array, then uses a callback function to 'filter' out the elements you need and then passes that data into a new array.

 While the `for()` loop is extremely versatile, it requires additional imperative programming to filter out elements of an array based on a condition and then place that data into a new array if needed. Using the `filter()` method is more functional and simply requires the condition to be engineered.

```js
const newArray = arr.filter(callback[, thisArg])
```

When the function is passed through the `filter()` method, it maintains the iterative element of the array and the index as well. You can think of it this way.

```js
function (el, index, arr)
```

The `filter()` method is considered a higher-order function simply because it requires a callback(function) as an argument. A secondary argument, `thisArg` is an optional parameter to use as `this` when executing the callback.

## Simple array filter

Before we get too far into complex examples of how the `filter()` method works with functions, I want to point out that you do not need to create additional functions and complex filter methods to get this to work. Take the following example. Here we have a range of numbers in an array assigned to the `inputData` variable.

Next, we have the `rangeData` variable that is performing a `filter()` method on the array object. Also, note that this function is completely inline using the new Fat Arrow syntax.

Notice `number` is the argument of the of the arrow function, I could have wrapped that in parens `()` if I had wanted and it would work. On the other side of the arrow `=>` `number` will maintain the value of the iterator of the `filter()` as it runs the condition.

```js
const inputData = [1, 44, 5694, 6, 12, 16, -12, 15, 22, 74, 18, 10];

const rangeData = inputData.filter(number => number >= 4 && number <= 40).sort();
```

In this example, the code is already executed and the new array that fits the condition of the filter is assigned to the variable.

```js
console.log(rageData) // [10, 12, 15, 16, 18, 22, 6]
```

Another great example is using the `filter()` method to find the odd numbers in an array. Illustrated in the example below I have an array of random numbers.

```js
const myArray = [];

for (let i = 0; i < Math.floor(Math.random() * 100); i++) {
  myArray.push(Math.floor(Math.random() * 100))
}

// build function to test if number in array
// is odd or not, set boolean value
function getOddNumbers(num) {
  return num % 2 !== 0;
}

// console log var value
console.log(myArray);
console.log(myArray.filter(getOddNumbers));
```


## Multi-pass filter on an array

Seeing how the `filter()` method can easily 'filter' out elements from an array and return a new array that can be used in any other matter, let's look at an example with slightly more complexity. For example, let's imagine that we have an array of data that is dirty, there are not only numbers but strings as well.

```js
const inputData = [1, 44, 5694, 6, 12, '15', 16, 'eighteen', -12, 'ten', 'number', 15, 22, 74, 18, 10];
```

We need our filter to do two things; 1) filter out all the non-number data, and 2) return only the numbers that fit within a specified range. For our range, we will use a simple object literal to define the min and max of this range.

```js
const range = {
  minimum: 10,
  maximum: 50
}
```

If you have a history of using JavaScript, the following `for()` loop imperative approach would be what you expect. In the following example first-order function we see the familiar code. An empty array, the `for()` loop iterating over the array passed into the function, the `if()` statement describing the condition to 'filter' out the non-number data, then a nested condition to evaluate if the value is within the range as described by the `range` object.

The elements that pass these conditions will be passed into the empty array, and finally, we return the data from the new array.

```js
function checkNumericRange(data, arg) {
  let newArray = [];

  for (let i = 0; i < data.length; i++) {
    if (typeof data[i] == 'number') {
      if (data[i] >= arg.minimum && data[i] <= arg.maximum) {
        newArray.push(data[i])
      }
    }
  }

  return newArray;
}

```

There should be nothing new here, and to execute this code we simply run the function and pass in the array and the object as arguments.

```js
const result = checkNumericRange(inputData, range).sort()

console.log(result) // [10, 12, 15, 16, 18, 22, 44]
```

In contrast, using the `filter()` higher-order function, we can remove the secondary looping of our previous function and simply make the range filtering part of the return statement. Notice that there is no loop here. When this function is passed into the `filter()` method, the looping is implied simply by the use of the `filter()` function. What we have left if the inner-workings of the implied loop.

Since I am filtering out two conditions, the code will use a single `if()` statement. There is no need to keep track of the iteration index. If any element in the array does not past the `number` condition, it returns `false` and then the `filter()` moves onto the next element in the array. If the element is a number, then the last condition is addressed in the return.

If we did not have the condition of the `number`, then we could simply use the return statement in the function.

```js
function checkNumericRange(el) {
  if (typeof el !== 'number') {
    return false;
  } else {
    return el >= this.minimum && el <= this.maximum;
  }
}
```

What is important to understand how a filter function works is that you never pass in an argument to address its parameter. The argument of the array is addressed by performing this method on the array object itself.

Instead of passing in the array as an argument to the function, you apply the filter higher-order function to the array object itself and in that method call you pass in the filter function, and in this case, an object reference for `thisArg`. Again, using the `sort()` method on the return gives us a nicely sorted array.

```js
const result = inputData.filter(checkNumericRange, range).sort();

console.log(result); // [10, 12, 15, 16, 18, 22, 44]
```

## Filter on an array of objects

Filtering on a single array is one thing, but about a more complex array, say an array of objects? For example, let's say that we have an array of objects with two properties each describing a type of vehicle.

```js
const vehicles = [
  {
    'make': 'Toyota', 'model': 'Sienna'
  }, {
    'make': 'Toyota', 'model': 'Rav4'
  }, {
    'make': 'Honda', 'model': 'CR-V'
  }, {
    'make': 'Honda', 'model': 'Civic'
  }, {
    'make': 'Ford', 'model': 'F-150'
  }, {
    'make': 'Honda', 'model': 'Accord'
  }, {
    'make': 'Subaru', 'model': 'Forester'
  }, {
    'make': 'Ford', 'model': 'Mustang'
  }
]
```

For the filter, we have a simple object that describes the type of vehicle we want.

```js
const vehicle = {make: 'Ford'}
```

Without using a `filter()` method we can do the same work again creating a function and using a `for()` loop. See in the example we have the `printVehicles()` function that has two parameters, one for the array and the other for the object we want to filter on. From there we loop through the array of objects looping in each object literal for the one that fits our condition.

```js
function printVehicles(arr, obj) {
  let newArray = [];

  for (let i = 0; i < arr.length; i++) {
    if (arr[i].make === obj.make) {
        newArray.push(arr[i]);
    }
  }
  return newArray;
}
```

Nothing wrong with this code. To execute we do the following.

```js
const auto = printVehicles(vehicles, vehicle)

console.log(auto)
```

And our return is

```js
[[object Object] {
  make: "Ford",
  model: "F-150"
}, [object Object] {
  make: "Ford",
  model: "Mustang"
}]
```

Pretty straightforward and makes sense if you know how to read procedural code. Now, let's do the exact same work, except using the `filter()` method and a lot less code. Here we have the `printVehicles()` function again, except this time there is no `for()` loop and our condition are simple. When the element `el` of the array we are using the filter on matches the value from our `thisArg` object, then return that value to an array.

```js
function printVehicles(el) {
  return el.make === this.make;
}
```

Using this function is much like the previous `printVehicles()` function, except this time we use this as an argument on the `filter()` method being applied to the array itself.

```js
const auto = vehicles.filter(printVehicles, vehicle)

console.log(auto)
```

The return is the same

```js
[[object Object] {
  make: "Ford",
  model: "F-150"
}, [object Object] {
  make: "Ford",
  model: "Mustang"
}]
```

At this point, you should see the power of the `filter()` method has when used on an array. Along with the functional programming paradigm, we have created simpler and reusable code that is less prone to bugs and declarative in its use.
