> {{book.title}} v{{book.version}}

# Repeating tasks with loops

Much of the code we write is repetitive. Now, there are two things you can do. One, you can repeat the code over and over and over and over until all the conditions are met, or you can program a robot to do your work. Like most, I am sure you are opting for the robot.

There a few different kinds of loops to use in different situations. The `for()` loop is for sure the most common. Then there is the `while()` loop, and last we will talk about the `do ... while()` loop.

Setting up any one of the loops is pretty simple. Set the statement, create a condition for the loop to run in and then while the condition is `true` perform an action. For example, the `for()` loop.

```js
for ( ... ) {
  ...
}
```

## 3, 2, 1 ... BLAST OFF!

This example makes no sense in real life, but it's an example of how to use an array of numbers and a `for()` loops. Now, I could write the same statement 10 times in a row and it will run, but that would be a real waste of time and effort. And I am so lazy that I don't even want to populate a counter either. That's what robots are for.

Before we get too deep, we should clearly understand the inner workings of a `for()` loop. Aside from the `for( ... ) { ... }` structure, there is the conditional statement inside the parens `()`. There are the three parts. One, set the variable `let i = 0`. You can set `i` to anything, but this is where the counting of the loop will start. Oh, and `i` is only a convention for `index`. You can make that anything you like.

Next there is the while `i < [value]` statement. This is instructing the loop to keep going as long `i` is less than the condition. In this example we want the loop to iterate over the elements in an array, so the easiest way to address this is to ask for the length of the array as a value.

Last, there is the index increment `i++`. With each pass of the loop, `i` will increase by `1`.

Let's step through the code that we will use in this countdown example. First I am setting two variables, one for an empty array that will be populated by a `for()` loop, and another variable that will set the max number of times I want my loops to run.

```js
const counter = []
const count = 10;
```

Next, I have my first `for()` loop. This will loop as many times as instructed by the `count` variable and with each pass add an index to the `counter` array. I am using the `unshift()` method to add to the array in reverse order so that we get that nice 10 - 1 countdown.

```js
for (let i = 0; i <= count; i++) {
  counter.unshift(i); // [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
}
```

For this next loop, I am going to make use of the `counter` array I created in the previous loop, but inside this one, I am also using an IIFE that captured the value of `i` and sets that for the value of the `setTimeout` function nested inside. This will give us that nice `3 ... 2 ... 1 ...` countdown feel.

With each return, the loop will log the value of the counter index in a string to the console.

```js
for(let i = 0; i < counter.length; i++) {
  (function (i) {
    setTimeout(function() {
      console.log(`${counter[i]} second(s) to blast off!`);
    }, i * 1000);
  })(i);
}
```

There you have it. We now have code that will do all the work for you. All you need to do is populate a single variable with a value and it's off and running. This has real-world implications too. Imagine that you have an interface where the user can set the value of `count` and then the counter runs in the browser. That's way more interesting than just pre-defining that variable for the user.

```js
const counter = []
const count = 10;

for (let i = 0; i <= count; i++) {
  counter.unshift(i); // [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
}

for(let i = 0; i < counter.length; i++) {
  (function (i) {
    setTimeout(function() {
      console.log(`${counter[i]} second(s) to blast off!`);
    }, i * 1000);
  })(i);
}
```

#### How does this code actually work?

An interesting side note, counters feel like the code is running in the background, waiting for a period of time and then doing something. In reality, all the code has run, but the `setTimeout()` function is an async function. This means that the value of time, `i * 1000` in this example, is all calculated before this code ever returns anything to the console.

What does that even mean? The JavaScript engine is not logging a value, then waiting a second and then logging another value. The code runs in an instant and creates a statement to be logged back to the console with a calculated wait time. Remember, `i` is not the value of an element in the array, but its index value. If we were to place a `console.log(i * 1000)` within the IFFE function, it would instantly return the following:

```js
0
1000
2000
3000
4000
5000
6000
7000
8000
9000
10000
```

What this means is ...

```js
"10 second(s) to blast off!" // is returned at 0 milliseconds
"9 second(s) to blast off!" // is returned at 1000 milliseconds in the future
"8 second(s) to blast off!" // is returned at 2000 milliseconds in the future
"7 second(s) to blast off!" // is returned at 3000 milliseconds in the future
...
"0 second(s) to blast off!" // is returned at 10000 milliseconds in the future
```

This async return sets a timed value return event in the future from once the code first ran.

## The while() loop

Now the `while()` loop, can work much in the way of the `for()` loop, but you need to set the iterator manually and this is usually set against a variable in an outer scope and looks something similar to this:

```js
let i = 0

while(i < 10) {
  console.log(i)

  i += 1;
}
```

While that works, I personally don't see the value in using this loop when the `for()` loop is better suited for this. But there are cases where we are not iterating over a fixed set of indexes, but are basically in an infinite loop until a condition is met. For example, imagine a scenario where your app is requesting a username. The username cannot be bypassed. What we want to say is, "_While this condition is `true` keep looping._"

This is a little confusing as the `true` statement is using reverse logic as `true` means that the `userName` variable has not yet been set. Once it is set, the `if()` statement will `break` the loop.

```js
while(true) {
  const userName = prompt("Please enter a User Name.");

  if (userName){
    alert(`Welcome ${userName}, do you want to play a game?`);
    break;
  } else {
    alert("Sorry, you must enter a User Name to play.");
  }
}
```

## The do ... while() loop

The last loop we will look at here is the `do ... while()` loop. For the most part, this works much like the `while()` loop, except that it always runs at least once before failing if the condition is `false`. Take the following example, setting an outer-scope variable to an integer less than the top end of the `while()` condition ensures that the loop will run.

```js
let i = 0;

do {
  console.log("The number is " + i);
  i++;
}
while (i < 10);
```

But, let's imagine that the value of `i` is outside the scope of the `while()` condition. Using a standard `while()` loop, this would not return any value. This is called a silent failure. To get around this you would need to build in a series of catches, probably using `if()` statements.

```js
let i = 11

while(i < 10) {
  console.log(i)

  i += 1;
}
```

But a better way to get around this is to use the `do ... while()` loop as illustrated in the following example.

```js
let i = 11;

do {
  console.log(`The number is ${i}`);
  i++;
}
while (i < 10);
```

Running this code, you will notice that the loop ran once and logged the value of `i` before breaking.
