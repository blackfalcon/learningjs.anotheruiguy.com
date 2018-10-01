> {{book.title}} v{{book.version}}

# Simplified function syntax

One of the more popular aspects of the new arrow function syntax is the simplification of how you can write a function expression. The basic syntax is as follows.

```js
(param1, param2, …, paramN) => { statements }
(param1, param2, …, paramN) => expression
// equivalent to: (param1, param2, …, paramN) => { return expression; }

// Parentheses are optional when there's only one parameter name:
(singleParam) => { statements }
singleParam => { statements }
singleParam => expression


// The parameter list for a function with no parameters should be written with a pair of parentheses.
() => { statements }
```

You will see that example repeated in a thousand blogs, but what does it really mean? To best explain this is to illustrate a function expression from it's most verbose to its simplest arrow function. For example, let's build a function with three parameters that returns the cubed value.

```js
const cubed = function(w, l, h) {
  return w * l * h;
}
```

The first simplification is to remove the reference of `function` and use the fat arrow `=>` on the other side of the parameter parens.

```js
const cubed = (w, l, h) => {
  return w * l * h;
}
```

The next simplification is to remove the curly brackets `{}` and simply reference the return on the other side of the fat arrow, sans the `return` keyword.

```js
const cubed = (w, l, h) => w * l * h
```

In all examples, the function would be executed the same.

```js
console.log(cubed(12, 8, 10))
```

If you are building a function with a single parameter, it gets even simpler.

```js
const foo = arg => arg
```

If you are building a function that does not take an argument, you still need to use an empty set of parens `()`.

```js
const foo = () => return
```

To increase complexity a little, let's explore a closure function and see how this boils down using the fat arrow syntax. In this example, the outer function has a single parameter, then there is an enclosed function that will access the outer scope variable and append that to its own parameter for a return.

```js
const foo = function(arg) {
  const bar = arg + ' - '

  return function(arg) {
    return bar + arg
  }
}

const baz = foo('outer');
console.log(baz('inner'));

// Curry style function
console.log(foo('outer')('inner'))
```

To reduce this to a simpler fat arrow syntax we can start to remove code and get the same result. Get rid of `function` keywords, unnecessary parens `()` and even curly brackets `{}` for enclosed functions.

```js
const foo = arg => {
  const bar = arg + ' - '

  return arg => bar + arg
}

const baz = foo('outer');
console.log(baz('inner'));

// Curry style function
console.log(foo('outer')('inner'))
```
