> {{book.title}} v{{book.version}}

# Curry functions

Currying is NOT a native JavaScript function. It is native to languages like Haskell and has been adapted to JavaScript using the concept of closures.

What is currying and why would we need it? To build functions that break up large argument structures so that the code is less complex. Using the curry technique, you can start to build argument structures that are more representational of the functions contained within the larger context of the closure.

Words ... I know, right? What does this look like? For starters, let's use a really simple example. Start by creating a standard function with a single argument. Then, the return of that function is another function with another single argument. Inside that function, we will do a simple return that combines the values of the first two arguments.

```js
function foo(a) {
  return function(b) {
    return a + b;
  }
}
```

When looking at this closure, while we pass in an argument for `a`, the return of this function is the anonymous function within.

```js
// execute
console.log(foo(5));


// return
function (b) {
  return a + b;
}
```

To complete this function, we could use a closure pattern ...

```js
const bar = foo(5);
console.log(bar(10)) // 15
```

Or, we could use a curry pattern. What currying allows us to do is reference the function and pass in the two arguments in a single execution by paring up the parens `()`.

```js
const bar = foo(5)(10);
console.log(bar) // 15
```

### Madlibs example

Given the examples so far this seems pretty straightforward, let's look at another example. This time I am going to take a reverse perspective and show the answer first as if it were an interview question.

> We are looking for a function that will take three separate arguments and output a Madlib phrase. "Pizza was invented by an 'adjective' 'nationality' chef named 'person'.

Coupled with this specification is an envisioned function that will be used. From this, we should be able to quickly see that this is a curry function call with a closure.

```js
madLibs(adjective)(nationality)(person);
```

Given what we learned before, when currying you can create a function with a single parameter and returns a function that has the next parameter, and so on. So the first function we need to create would be the function accepts the first argument for the `adjective`.

```js
function madLibs(adjective) {
  ...
}
```

Great. Next, we need this function to return the next function in out Madlib and that would be the `nationality` question.

```js
function madLibs(adjective) {
  return function(nationality) {
    ...
  }
}
```

Awesome, now that we have the second step in our curry function completed, we need to return yet another function, that of the `person` question.

```js
function madLibs(adjective) {
  return function(nationality) {
    return function(person) {
      ...
    }
  }
}
```

There, we have our last step in this curry function. The final step is to build the return statement that will output our Madlib string.

```js
function madLibs(adjective) {
  return function(nationality) {
    return function(person) {
      console.log(`Pizza was invented by an ${adjective} ${nationality} chef named ${person}.`);
    }
  }
}
```

Last, but not least, let's create our MadLib.

```js
madLibs('apologetic')('Canadian')('Tom');
```

And in the console, you should see ...

```
"Pizza was invented by an apologetic Canadian chef named Tom."
```

Awesome!
