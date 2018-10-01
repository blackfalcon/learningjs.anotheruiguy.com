> {{book.title}} v{{book.version}}

# Variables

> The variable statement declares a variable, optionally initializing it to a value.

Before we get too far into things, let's cover some basics around JavaScript variables. First, there is the concept of `scope`, which we will dive deeper into later, and then there are some complex ideas around `hoisting` and the newly improved variable scoping concepts in ES6.

## Basic variable use

Creating a variable in JavaScript is very simple. You can simply write a name in your code, add the equal `=` operator, a value, and you have a variable with an assigned value.

```js
foo = 8 * 10
console.log(foo) // 80
```

There is nothing wrong with this code, but there will be issues with scope and value mutation. In the following example see how easy it is to not only change the value of a variable, but it's primitive type as well. The mutation of type is because JavaScript variable types are not defined at the variable's declaration, but are dynamically typed at initialization.

```js
foo = 8 * 10
console.log(foo) // 80
console.log(typeof foo) // "number"

foo = 'foo'
console.log(foo) // "foo"
console.log(typeof foo) // "string"
```

The previous example illustrates an undeclared global variable statement. What is most common is to have a variable declared using the `var` keyword. Variable declaration is a key part of understanding the concept of `hoisting`, more on that later. In the end, using the `var` keyword to declare a variable has little effect on the variable itself in this general/global context.

```js
var foo = 8 * 10
console.log(foo) // 80
console.log(typeof foo) // "number"

var foo = 'foo'
console.log(foo) // "foo"
console.log(typeof foo) // "string"
```

In JavaScript, there are three types of variable scoping; the first two are global and functional. The third is block scope, introduced in ES6.

The previous example illustrated JavaScript's global context. Functional scope is simply a variable inside a function. Using a simple function, we see how, even when declaring the variable with the `var` keyword, `var foo` inside the `bar()` function does not pollute the global scope.

Using the same name of a global variable inside the scope of a function is a common practice called **variable shadowing**.

```js
var foo = 8 * 10
console.log(foo) // 80

var foo = 'foo'
console.log(foo) // "foo"

function bar() {
  var foo = [1, 2, 3];
  return foo
};

console.log(bar()) // [1, 2, 3]
console.log(foo) // "foo"
```

If the `var` keyword is removed from the `foo` variable inside the scope of the function, this will allow the value of the nested `foo` variable to mutate the value of the global `foo` variable.

```js
var foo = 8 * 10
console.log(foo) // 80

var foo = 'foo'
console.log(foo) // "foo"

function bar() {
  foo = [1, 2, 3]; // breaks out of functional scope, pollutes global scope
  return foo
};

console.log(bar()) // [1, 2, 3]
console.log(foo) // [1, 2, 3]
```

It's important to note that removing the `var` keyword does not always go all the way to the outermost global scope. Functions have an interesting way of wrapping code and defining what is called its `parent` scope. The following code example illustrates this rather well. There is an outermost global variable, that in this example would be assigned to the `window` scope. Next, there is a function and inside that is another function. This pattern is called a `closure`, more on this later.

What we can see here is that when removing the `var` keyword from inside the function `baz()`, this will have an impact on its parent scope/function, changing the value from `function` to `closure` and stops. The outermost `foo` value is still `global`.

```js
var foo = "global";

(function bar() {
  var foo = 'function';

  (function baz() {
    foo = 'closure';
    console.log(foo); // closure
  })();

  console.log(foo); // closure
})();

console.log(foo); // global
```

## Hoisting

Variable hoisting is the perception of what JavaScript engines do in the background when it runs the code. JavaScript takes two passes at the code. The first pass is to load all the variables and functions into memory. The second pass is to actually run the code. In the following example if one were to try to `console.log(x)` before `x` is even defined, then the engine will return `undefined`. If the value of `x` is logged after the value is initialized, then this will return the value of the variable.

If there is an attempt to `console.log(y)` without declaring or initializing it, the JavaScript engine will return an error.

```js
console.log(x); // -> undefined
var x = 5;
console.log(x); // -> 5
console.log(y); // -> error (no var declared)
```

The reason is, the JavaScript engine breaks apart the variable declaration in memory and looks something like this ...

```js
var x;
console.log(x); // -> undefined (var x has no value)
x = 5;
console.log(x); // -> 5
console.log(y); // -> error (no var declared)
```

The variable `x` actually exists BEFORE `console.log(x)`, but it doesn't yet have a value.

### What about inside functions?

Hoisting is not much different within the scope of a function. The keyword here being `scope`. In the following example, `var y` is defined in the global scope. So within the scope of `function foo()` when `console.log(y)` is fired there is a valid value for that variable.

```js
var y = 10;

(function foo() {
  console.log(x); // -> undefined
  var x = 5;
  console.log(x); // -> 5
  console.log(y); // -> 10 (taken from global scope)
})();
```

When a function is setting a variable, it may have impact on it's parent scope. Here, when a function is looking to get the value of a variable, it will look in it's immediate scope, if not there, it will look in its parent scope. Unlike setting a variable, when looking for a value, JavaScript will keep looking until its exceeded the scope of the outermost parent. Take the following code for example.

```js
var y = 10;

(function bart() {
  (function baz() {
    (function foo() {
      console.log(x); // -> undefined
      var x = 5;
      console.log(x); // -> 5 (foo() scope)
      console.log(y); // -> 10 (global scope)
    })();
  })();
})();
```

### What about functions themselves?

Functions are also hoisted. This is a pretty powerful concept to understand as it's possible to reference a function EVEN BEFORE it has been declared in the JavaScript. In the following example, the JavaScript engine hoisted the function `hoisted()` above that of the use of `hoisted()` so it will run.

```js
hoisted();

function hoisted() {
  console.log('this works');
}
```

BUT WAIT! Anonymous functions work much like variable declarations. In the following example using an anonymous function, `var notHoisted` is declared, but not yet defined.

```js
notHoisted();

var notHoisted = () => {
  console.log('this does not work');
}
```

So calling the `notHoisted()` function prior to definition will cause an error `TypeError: notHoisted is not a function`.

If we simply move the reference to the `notHoisted();` to after the function definition, it will work.

```js
var notHoisted = () => {
  console.log('YAY! This works now!');
}

notHoisted();
```

It is because of these hoisting rules that more developers prefer anonymous functions. These rules ensure that a function will only run in its immediate context and not accidentally be available too soon in the code.
