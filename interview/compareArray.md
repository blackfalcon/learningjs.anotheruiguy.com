# Compare the array to find the sum

The task before you is; you are given an array of numbers and a single sum total integer. The goal is to build a function that will parse through the array and find numbers that add up to the sum integer.

This is given:

```js
var arr = [5, 2, 3, 1, 4, 9, 6, 8, 7];
var sum = 10;

function parseArray(arr, sum) {
  ...
}
```

### Talk through the solution

To start off, we know that we need a function that will take two arguments, `arr` and `sum`.

```js
function parseArray(arr, sum) {

  // your answer will go here

};
```

In order to make this happen, it is assumed that we will need a few things.

1. It's probably most efficient to loop through the array only once, but to do so, we need to loop from two directions at the same time
1. We may need to pre-sort the array so that the integers are in good order
1. Once we have established how we will move through the array, we can probably use a `while` loop
1. Next we need a way to compare each selected integer and evaluate them against our `sum` value
1. Last we need to put these found pairs into an object that we can return

Initial variables we will need are ...

```js
function parseArray(arr, sum) {

  // this will be a value that will get updated with each while loop
  var left = 0;

  // again, this value will get updated with each while loop
  var right = arr.length - 1;

  // pre-sort the array
  var sortedArr = arr.sort();

  // empty object for our answer
  var returnedObject = {};
};

```

Now we can establish the `while` loop that will compare the `left` with the `right` values as long as they are not equal.

```js
function parseArray(arr, sum) {
  var left = 0;
  var right = arr.length - 1;
  var sortedArr = arr.sort();
  var returnedObject = {};

  while(left != right) {
    // ....
  }
};

```

Within the loop we need to capture the actual integers that we are adding up and comparing to the `sum` argument. Since we already used `left` and `right`, let's call these `start` and `end`.

Remember that we are not using the array as is, but we are using a pre-sorted version that is captured in the ` sortedArray` variable. Within the scope of the `while` loop, we need to use the value of `left` and `right` to determine our `start` and `end` values, like so;

```js
function parseArray(arr, sum) {
  var left = 0;
  var right = arr.length - 1;
  var sortedArr = arr.sort();
  var returnedObject = {};

  while(left != right) {
    var start = sortedArr[left];
    var end = sortedArr[right];
  }
};
```

Then we need to place the sum total of those numbers into a variable that we can use called `calculatedSum`. This will be the sum total of the `start` and `end` values.

```js
function parseArray(arr, sum) {
  var left = 0;
  var right = arr.length - 1;
  var sortedArr = arr.sort();
  var returnedObject = {};

  while(left != right) {
    var start = sortedArr[left];
    var end = sortedArr[right];
    var calculatedSum = start + end;
  }
};
```

Next we need to compare the `calculatedSum` against the value of the `sum` argument, and to do that we will use a basic `if()` statement. We also need to need to account for when the `calculatedSum` does not equal the `sum` value.

```js
function parseArray(arr, sum) {
  var left = 0;
  var right = arr.length - 1;
  var sortedArr = arr.sort();
  var returnedObject = {};

  while(left != right) {
    var start = sortedArr[left];
    var end = sortedArr[right];
    var calculatedSum = start + end;

    if (calculatedSum === sum) {
        ...
    } else if (calculatedSum != sum) {
      ...
    }
  }
};

```

Within the scope of this `if()`, if the return is `true`, then we need to add the integers we found into our new object and then update the values of `left` and `right` which will also update the values of `start` and `end` as well.

Left, of course, will loop forward while right, will loop in reverse. Remember, we are looping from both ends of the array.

```js
function parseArray(arr, sum) {
  var left = 0;
  var right = arr.length - 1;
  var sortedArr = arr.sort();
  var returnedObject = {};

  while(left != right) {
    var start = sortedArr[left];
    var end = sortedArr[right];
    var calculatedSum = start + end;

    if (calculatedSum === sum) {
      returnedObject[start + '-' + end] = true;
      left++;
      right--;
    } else if (calculatedSum != sum) {
      ...
    }
  }
};
```

When the `calculatedSum` does not equal the `sum` value, then we only need to loop the `right` value one step back and see if that number plus the previous `left` number will equal the `sum` value.

```js
function parseArray(arr, sum) {
  var left = 0;
  var right = arr.length - 1;
  var sortedArr = arr.sort();
  var returnedObject = {};

  while(left != right) {
    var start = sortedArr[left];
    var end = sortedArr[right];
    var calculatedSum = start + end;

    if (calculatedSum === sum) {
      returnedObject[start + '-' + end] = true;
      left++;
      right--;
    } else if (calculatedSum != sum) {
      right--;
    }
  }
};
```

The final step for this to wok is that we need to log the value of the new `returnedObject` we have been creating through this `while` loop.

And to make this work, we use our given array and sum and load this into the new `parseArray()` function.

```js
var myArray = [5, 2, 3, 1, 4, 9, 6, 8, 7];

parseArray(myArray, 10);
```

And there you have it. The return in the console should look something like ...

```js
[object Object] {
  1-9: true,
  2-8: true,
  3-7: true,
  4-6: true
}
```
