# Currying

First things first, currying is NOT a native JavaScript function. It is native to languages like Haskell, but has been adapted to JavaScript using the concept of closures.

But what is currying and why would we need it? The basic concept is, build functions that break up large argument structures so that the code is less complex. Using the curry technique, you can start to build argument structures that are more representational of the functions contained within the larger context of the closure.

Words ... I know, right? What does this look like. For starters, let's use a really simple example. Start by creating a standard function with a single argument. Then, the return of that function is another function with another single argument. Inside that function, we will do a simple return that combines the values of the first two arguments.

```js
function foo(a) {
  return function bar(b) {
    return a + b;
  }
}
```

In this example, we need to execute a function that passes in the two arguments inside individual parens `()`, we never call `bar()` directly as that is the return of `foo()`, we do this like so ...

```js
var baz = foo(5)(5);
console.log(baz) // 10
```

This example makes sense, but what if we only pass in one of the paren arguments? What gets returned? Well, since the curried function is not resolved, it returns the function. For example, let's run the following code.

```js
var baz = foo(5);
console.log(baz);
```

The return is ...

```js
function (b) {
  return a + b;
}
```

If we added a `console.log()` before the return, we would see that the `a` variable is being set. So, how can use this to our advantage? Take a look at the example below. By the laws of JavaScript physics we can create a variable and store the value of `a` and then the remainder of the function. So then, we can actually take this new variable and pass in the last argument for `b` and then log the return.

```js
var baz = foo(5);
var fooBar = baz(5)
console.log(fooBar); // 10
```

So what's interesting here is that we can use this function and only address part of it's arguments and store that in a variable and carry the rest of the function with us in that variable. Then when we have the next argument available to us, we can pass that into our stored variables and complete the function's return.

### Another example

Although this seems pretty straight forward ... mastering the Curry function is something that takes time. So, let's look at another example. This time I am going to take a reverse perspective and show the answer first as if it were an interview question.

> We are looking for a function that will take three separate arguments and output a Mad Lib phrase. "Pizza was invented by a 'adjective' 'nationality' chef named 'person'."

```js
madLibs(adjective)(nationality)(person);
```

Given what we learned before, when currying you can create a function that returns a function that has the next argument, and so on. So, the first function we need to create would be the function that is the next call in our answer, the `madLib()` function. And this will take the first argument for the `adjective`.

```js
function madLibs(adjective) {
  ...
}
```

Great. Next we need this function to return the next function in out Mad Lib and that would be the `nationality` question.

```js
function madLibs(adjective) {
  return function(nationality) {
    ...
  }
}
```

Awesome, now we have the second step in our curry function. Last we need to return yet another function, that of the `person` question.

```js
function madLibs(adjective) {
  return function(nationality) {
    return function(person) {
      ...
    }
  }
}
```

There, we have our last step in this curry function. Now we need the actual return statement that will output our Mad Lib question.

```js
function madLibs(adjective) {
  return function(nationality) {
    return function(person) {
      let str = `Pizza was invented by a ${adjective} ${nationality} chef named ${person}.`;
      console.log(str);
    }
  }
}
```

Last, but not least, let's create our MadLib.

```js
madLibs('apologetic')('Canadian')('Tom');
```

And in the console you should see ...

```
"Pizza was invented by a apologetic Canadian chef named Tom."
```

Awesome!
