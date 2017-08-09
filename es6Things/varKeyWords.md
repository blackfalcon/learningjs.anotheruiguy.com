# Var, Let and Const

What are the key difference between these variable creating keywords? Before we jump too far into this concept, let's be sure to understand what the different types of scope are.

There is the `global` or `parent` scope. This is the outer most scope of any JavaScript function. It could simply refer to the scope of a function or the `window` object.

Next there is the new BLOCK scope introduced in ES6. Simply put, a BLOCK is any function contained within curly brackets `{ ... }`. A good example of a JavaScript block is inside a `for()` loop. In the following example I have created a small loop that will log the value of `foo` with each iteration. I have `foo` defined in the outer/parent/global scope and then I have a final statement that will log the value of `foo` once again.

```js
var foo = 'foo';

for(e = 0; e < 3; e++) {
  var foo = 'bar';
  console.log(foo); // bar
}

console.log(foo); // bar
```

As expected, the value of `foo` is changed by redefining the variable inside the `for()` loop's block `{}`. Using the `let` keyword, this will actually maintain the scope of the block and not pollute the outer scope variable.

```js
let foo = 'foo';

for(e = 0; e < 3; e++) {
  let foo = 'bar';
  console.log(foo); // bar
}

console.log(foo); // foo
```

When we run this code, the output would suggest that within the scope of the `for()` loop, the value for `foo` is `bar`. And this has NO effect on the outer scope and the final log for `foo` is `foo` as expected.

Now that we understand what blocks are, let's dig a little deeper into these concepts.

## The VAR

The `var` keyword is as old as JavaScript itself. It's uses are legendary. But it's failure is it's lack of natural scoping properties. But it's not its fault, the concept of scope in JavaScript has matured with the language itself.

Just a quick refresher, using the `var` keyword will allow the value of that variable exist within it's context until the context is closed. If you crate a variable value in the `window.object` or global scope, this value will persist until that experience is closed.

To manage scoping in JavaScript, it's best to maintain your variables within the context of a `function()`. That way, the value for that variable is only created within that context and once the `function()` is closed, that value is tossed from memory, or [garbage collected](thingsToKnow/garbageCollection.html).

It is also common to see that once you have established a variable within the global scope or within the context of a function or closure, you then have the ability to change that value as you move through the individual actions of the function.

In the following example, I am declaring the `y` variable in the global scope. Next I am creating a self-executing function that will also set the variable of `y` to another string. And inside that, yet another self executing function that resets the value of `y`.

```js
var y = 'global';

(function foo() {
  var y = 'parent';

  (function bar() {
   var y = 'function scoped';
   console.log(y) // logs function scoped
  })();

  console.log(y) // logs parent
})();

console.log(y) // logs global
```

You see that the final logged return of `y` is the global value, but why? This is because the nested values of `y` are scoped inside their respective functions. What is also interesting is that we do not get an error that the var `y` has already been assigned. This again is proof that a variable's name and it's value are scoped within it's respective function.

Making this a little more interesting we can create a function that has a nested function, and a JavaScript block all using the `var` variable keyword.

Inside the block I am also repeating the `var alpha` assignment just to illustrate, but know that you will get an error because the var `alpha` has already been defined. So you could just use `alpha = ''` and this would work too, and that would really illustrate how a variable simply inside a block pollutes the outer scope of the function.

```js
function zap() {
  var alpha = 'parent scope';
  console.log(alpha); // parent scope

  function beta() {
    var alpha = 'closure scope';
    return console.log(alpha);
  }

  {
    var alpha = 'block scope';
    console.log(alpha); // block scope
  }

  console.log(alpha); // block scope
  return beta(); // closure scope
};

zap();
```

When you run this code, you will see the following output.

```js
"parent scope"
"block scope"
"block scope"
"closure scope"
```

The reason is, the `var alpha` reference is mutating the value of `alpha` set initially on line 2 of this function.

#### Test what you just learned ...

To test your understanding of the `var` keyword and how to maintain scope, try the following exercise.

1. crate the variable of `foo` and assign the value of `12`
1. crate the function of `bar` and inside create another variable of `foo`, but this time assign the value of `22`
1. execute the `bar()` function
1. console log the value of `foo`

{% exercise %}
{% initial %}
{% solution %}
var foo = 12;

function bar() {
  var foo = 22;
}

bar();
console.log(foo)

{% validation %}
var foo2 = 12;

function bar2() {
  var foo2 = 22;
}

bar2();
console.log(foo)

assert(foo === 12);
{% endexercise %}

## The LET

The `let` keyword, for the most part, is very similar to `var`, but there are some key differences. One being, as we have already discussed a little, is the new concept of `block` scope. The other difference is, the `let` variables DO NOT get [hoisted](/thingsToKnow/hoisting.html) like the `var` keyword does.

Using the same function we created with the `var` keyword, let's simply update the `var` keyword to `let` and see how this behaves. Before we jump into the code

```js
function pop() {
  let alpha = 'parent scope';
  console.log(alpha); // parent scope

  function beta() {
    let alpha = 'closure scope';
    return console.log(alpha);
  }

  {
    let alpha = 'block scope';
    console.log(alpha); // block scope
  }

  console.log(alpha); // parent scope
  return beta(); // closure scope
};

pop();
```

This time we see in the output that we have maintained contextual scope for all the variable uses. Variables set in the outer scope of the function are persisted as expected. The nested, or closure, scoped function acts as we would expect too. The thing to notice is that the variable reference within the block statement maintains its scope and does not pollute the outer scope of the function.

```js
"parent scope"
"block scope"
"parent scope"
"closure scope"
```

## The Const

In the previous examples we did a lot of data mutation of defined variables within their scope. But when we get to `const`, the main thing that we need to understand that the role that this keyword plays is that you are NEVER to try and reassign the value of a `const` variable with a new value.

To see this in action, here is a simple example where I am creating the variable of `i` and then reassigning it's value to `2` and then logging that value. In the next block of code I am using the `const` keyword. So when I try and change that variables value (say this was inside a for loop or something of that nature) I will get a `type` error returned. This telling me that I am trying to mutate the value of a variable that has been set as a constant.

```js
let i = 1;
i = 2;
console.log(i); // 2

const e = 1;
e = 2;
console.log(e); // type error
```

It is important to remember that while you can't directly change the `type` and `value` of a `const` assignment, you can modify its content. For example, in the case of creating an object using the `const` keyword. This is mainly saying that you want to maintain the `type` of the value.

```js
const foo = [1, 2, 3];

foo.push(4);

console.log(foo); // [1, 2, 3, 4]
```

This is not the same as reassigning the total value of the array. For example, the following code will return an error.

```js
const foo = [1, 2, 3];

foo = ['a', 'b', 'c']

console.log(foo); // TypeError: Assignment to constant variable.
```
