> {{book.title}} v{{book.version}}

# Closure functions

> A closure is the combination of a function and the lexical environment within which that function was declared.

The primary functionality that a closure is trying to address is the management of variables within the scope of a function. Before we get too deep it is important to remember that a function has access to variables within its specific scope, but also its parent scope. That means it will keep bubbling up looking for a resolution to a variable before it reports `undefined`. Meaning, if you are using globally scoped variables that are represented in your function, nothing is sacred.

## Lexical scope

When it comes to writing a function that fits the definition of a closure, there are two scopes to be aware of. The Closure scope and the Lexical scope. What is the difference? For starters let's look at a lexical scope function.

In this example, we have the `lexical()` function with two parameters. We can execute this function and all its parameters in a single statement, `lexical(arg, arg)`. What makes this interesting is that the nested function `mySidekick()`, and this has to be a named function in this example, has access to the `concatString` variable in the parent scope and the `newStr` variable in its immediate scope.

In the `return` statement you need to reference the nested function with the second argument passed into its parameter.

With a lexically scoped function, calling `lexical()` is requesting that the return of the function comes back and without passing in any arguments we simply get a string return with `undefined` elements in the string.

```js
function lexical(string, strTwo) {
  const concatString = `Hi, I am ${string}.`;

  function mySidekick(sidekick) {
    const newStr = `And this is my sidekick ${sidekick}`;
    return `${concatString} ${newStr}`;
  }

  return mySidekick(strTwo);
}

console.log(lexical()) // "Hi, I am undefined. And this is my sidekick undefined"
console.log(lexical('Cpt. America', 'Bucky')) // "Hi, I am Cpt. America. And this is my sidekick Bucky"
```

## Closure scope

A closure scoped function is not too different from that of a lexically scoped function. What's interesting to note is that in the `return` statement we are returning the function itself, not its return and that's important to understand.

```js
function closure(string) {
  const concatString = `Hi, I am ${string}.`;

  function mySidekick(sidekick) {
    const newStr = `And this is my sidekick ${sidekick}`;
    return `${concatString} ${newStr}`;
  }

  return mySidekick;
}
```

The primary reason this is important is if we simply `console.log(closure())` we get the enclosed function as the return. What that means is that we can't simply call this function as we did with the lexical function `closure('Batman')`. What we can do is create a new variable and pass in the first argument, this will return the closure function with access to the outer scope value. Like so;

```js
const batMan = closure('Batman')
```

At this point, we can't simply `console.log(batMan)` as this will still return the closure function incomplete. As a final step, we can call the new variable as a function and pass in the missing argument.

```js
const batMan = closure('Batman')
const capedCrusaders = batMan('Robin')
console.log(capedCrusaders) // "Hi, I am Batman. And this is my sidekick Robin"
```

Before I close this out, I want to illustrate that since a closure function simply returns the enclosed function and not its return, you don't need to name it, but you can simply return an anonymous function instead. In the following example, I updated the return statement to be the function itself versus having to name it and return it.

```js
function closure(string) {
  const concatString = `Hi, I am ${string}.`;

  return function(sidekick) {
    const newStr = `And this is my sidekick ${sidekick}`;
    return `${concatString} ${newStr}`;
  }
}
```

#### It's a closure, but you can curry it?

We will discuss what a curry function is and how it can be used in the next section, but in this running example, I want to illustrate how a closure can be used with a curry function pattern and get the same results. Looking at the following example this is exactly the same as the `closure()` function. All that will be different is how we access this function and its enclosed feature.

```js
function curry(string) {
  const concatString = `Hi, I am ${string}.`;

  return function(sidekick) {
    const newStr = `And this is my sidekick ${sidekick}`;
    return `${concatString} ${newStr}`;
  }
}
```

For starters, if I simply `console.log(curry()` I get a return of the inner function. Much like a curry function. And EXACTLY like a closure function I can define a variable and assign it the `curry()` function with the first argument addressed. Following that we can again take that variable we created and use it like a function and pass in the second argument. At this point nothing is different.

```js
const pilot = curry('Han');
const team = pilot('chewie');
console.log(team) // "Hi, I am Han. And this is my sidekick chewie"
```

What makes this example fit in the curry pattern is how we use it. When you curry a function you can group the arguments in a single reference. No, not like the lexical function, but by `function()()`.

```js
const team = curry('Han')('chewie');
console.log(team) // "Hi, I am Han. And this is my sidekick chewie"
```

There is much more to the idea of a curry function, but I simply wanted to point this out as there may be some who have noticed this already.
