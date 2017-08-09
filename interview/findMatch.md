# Find the match in the arrays

In all the JavaScript interviews I have come across, there is a common theme. "Do you loop bro?" While the question is not that specifically, the answer typically goes in that direction. For example;

> Let's say that I have two arrays, what I need is a function that will accept these two arrays and compare them to each other and produce a single array that only has the items that matched.

Seems pretty straight forward, right? On the surface, this almost seems more difficult than the other [problem](/interview/compareArray.html) of finding values in an array that equal the provided sum. But it's not. It's actually much simpler. Let's break this down into what an example would look like.

Let's say that our arrays look like this;

```js
array1 = [1, 2, 3, 4, 5, 6];
array2 = [2, 4, 6, 8, 10];
```

The answer we would be looking for is ...

```js
[2, 4, 6]
```

So, how do we code for this? For starters, let's create our function that will handle this work and call it `diff()`.

```js
function diff(a, b) {
  ...
}
```

In order for this to work, we will need to declare a new variable inside that function where we can build out our new array.

```js
function diff(a, b) {
  var matches = [];
}
```

For the next part of our magic trick, we will need to build a `for()` loop that will iterate through our first array.

```js
function diff(a, b) {
  var matches = [];

  for (i = 0; i < a.length; i++) {
    ...
  }
}
```

By assigning `a.length` to the scope of the `for()` loop, we are not consuming that argument's array and iterating on those items. Keep in mind, the `for()` loop at this stage has totally lost track of the actual items in the list and is simply running through `0, 1, 2, 3 ...`. We will need to address that later.

In order for this function to loop through the array items in the `b` argument, we need to nest another loop.

```js
function diff(a, b) {
  var matches = [];

  for (i = 0; i < a.length; i++) {
    for (e = 0; e < b.length; e++) {
      ...
    }
  }
}
```

This time, we are going to store the iterations in the `e` variable and iterate in the `b` argument. What is happening here? The outer loop grabbed the first item in the array and holds that as the value of `i`. Then while `i` has a value, the second loop iterates through every item in the `b` array using the `e` variable. It is in this scope we can then compare items from the `a` array to the `b` array.

It is here that we will put our compare logic and decisions/actions. Remember that `i` and `e` only have iteration values, not the actual value of the item in the array. In this next step we need to create our `if()` logic that compares these things.

```js
function diff(a, b) {
  var matches = [];

  for (i = 0; i < a.length; i++) {
    for (e = 0; e < b.length; e++) {
      if(a[i] === b[e]) {
        matches.push(a[i]);
      }
    }
  }
}
```

And that's it! Crazy huh? The last thing we need to make this work is a `return` statement to get the value of `matches` out if the function.

```js
function diff(a, b) {
  var matches = [];

  for (i = 0; i < a.length; i++) {
    for (e = 0; e < b.length; e++) {
      if(a[i] === b[e]) {
        matches.push(a[i]);
      }
    }
  }

  return matches;
}
```

With that in place, we can run the following code against the function.

```js
var listAlpha = [1, 2, 3, 4, 5, 6, 7, 8]
var listBeta = [2, 4, 6, 8, 10]

var foo = diff(listAlpha, listBeta);
```

And our result will be ...

```js
[2, 4, 6, 8]
```

Given this, we can throw literally anything into these arrays and we can compare their values.

### What about ES6 bro!

Ahh yes ... our new friend ES6. Well, for the most part, ES6 doesn't really change this solution too much, but we can use a new tool called the `for ... in` loop versus the traditional `for()` loop.

What's interesting with the `for ... in` loop is that instead of being very mechanical, it's easier to think of things in the plural versus singular naming conventions. In this example, we have `array` and then `nextArray`. Each `array` contains 1 or more item(s).

Whereas we had before in the `for()` loop, leveraging the iterator of `i`, we don't have that here. So when we break apart a plural form, we reference the singular item within it's array.

For example, `for (let item in items)` will return the iterator value of the array, `[0, 1, 2, ...]`. It's when we look for the specific `item` in `items` that we get the value of the iterator.

Let's create some new lists ...

```js
var thoseNames = ['Tom', 'Dick', 'Harry', 'Alice', 'Peter', 'Mike'];
var theirNames = ['Dale', 'Tom', 'Peter'];
```

Then we have our new `diff` function.


```js
function diff(array, nextArray) {
  var matches = [];

  for (let item in array) {
    for (let nextItem in nextArray) {
      if (array[item] === nextArray[nextItem]) {
        matches.push(array[item])
      }
    }
  }
  return matches;
}
```

We can now run this function like so;

```js
var foo = diff(thoseNames, theirNames);

console.log(foo);
```

And we get back ...

```js
["Tom", "Peter"]
```

Myself, I am a HUGE fan of the new `for ... in` loop versus the old standard `for()` loop. Hope you are too!
