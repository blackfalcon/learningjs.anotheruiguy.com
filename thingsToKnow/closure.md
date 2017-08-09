# Closures

Closures used to really confuse me, a lot. I mean, it's one of those JavaScript names that makes you think that it's something magical and mysterious. Or ... maybe it's just me.

### What is the literal definition of a Closure?

Simply put, a closure is a function that is within the context of another function. You can either physically embed the function within the parent function, or you can refer to an outer function via a [callback](/thingsToKnow/callbacks.html). The main goal of this technique is to maintain the context of [this](/thingsToKnow/this.html).

Another way of saying this is, a closure is nothing more than a functions ability to access a variable outside of its scope. It's using a variable that in the function's body.

What does that mean? In JavaScript having global [anything] is a REALLY BAD IDEA. Why? The main reason is data mutation. JavaScript is all about the vars and setting global scope has the potential to mutate the value of all those variables and cause you more stress then you can imagine! Remember, JavaScript is a Run Time language and it's damn fast, so you want to keep your scope limited so that you are not searching over thousands of lines of code to find your bug.

### Examples please!

```js
function foo(string) {
  const concatString = `I am ${string}`;
  const punc = '!';

  function fooBar() {
    console.log(concatString + punc);
  }

  return fooBar();
}

foo('Bat-Man');
```

Let's unpack this. First we have the function of `foo()` that takes `string` as an argument. Inside the `closure` is another function called `fooBar()`.

Within the closure, the function `fooBar()` will first look for the value of concatString within it's own scope, but then when not found it will look in it's parent's context.


### Is a Callback a Closure?

Yes. A closure is sometimes a callback as well, so in the following example, I updated the `fooBar()` function to be global, but is being called into the scope of `foo()`

```js
function fooBar(arg) {
  console.log(arg);
}

function foo(arg) {
  var concatString = `I am ${arg}`;
  const punc = '!';

  fooBar(concatString + punc);
}

foo('SUPERMAN');
```
