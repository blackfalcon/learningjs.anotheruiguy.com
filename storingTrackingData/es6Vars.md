> {{book.title}} v{{book.version}}

# ES6 variables; typing and scoping

At this point, there should be a clear understanding of the basics of variables and hoisting, and even some introduction to scoping. To take things to the next level, the latest specification on JavaSript, ES6, introduces new variable types and they come with new functional constraints.

### LET

The `let` keyword, for the most part, is very similar to `var`, but there are some key differences. The big one being, as we have already discussed a little, is the new concept of `block` scope, or anything inside curly brackets `{}`. The other difference is, the `let` variables DO NOT get hoisted like the `var` keyword does.

Referencing the hoisting examples in the previous module, the following example replaces the `var` keyword with `let`. This time the code errors immediately because the variable of `x` is not yet declared or defined prior to the `console.log()` function.

```js
console.log(x); // "ReferenceError: x is not defined
let x = 5;
console.log(x);
console.log(y);
```

For another example, here is a function that uses the `var` keyword, some nesting and a block for separation.

```js
function pop() {
  var alpha = 'parent scope';
  console.log('should display parent scope: ' + alpha); // parent scope

  (function beta() {
    var alpha = 'closure scope';
    console.log('should display closure scope: ' + alpha); // closure scope
  })();

  {
    var alpha = 'block scope';
    console.log('should display block scope: ' + alpha); // block scope
  }

  // Will display block scope because the previous
  // var alpha was NOT contained within the block scope
  // and over-wrote the var alpha = 'parent scope' declaration
  console.log('should display block scope: ' + alpha); // block scope
};

pop();
```

As illustrated, the variable value from inside the block bleeds out into the parent scope and mutates the first instance of `alpha`.

In the next set of code, the `var` keyword is updated to `let` inside the scope of the block and the result is now encapsulated to the scope of the block.

```js
function pop() {
  var alpha = 'parent scope';
  console.log('should display parent scope: ' + alpha); // parent scope

  (function beta() {
    var alpha = 'closure scope';
    console.log('should display closure scope: ' + alpha); // closure scope
  })();

  {
    let alpha = 'block scope';
    console.log('should display block scope: ' + alpha); // block scope
  }

  // The previous block scope variable value is scoped to the block
  // and does not mutate the parent value
  console.log('should display parent scope: ' + alpha); // parent scope
};

pop();
```

Variables set in the outer scope of the function are persisted as expected. The nested, or closure, scoped function acts as expected too. The thing to notice is that the variable reference within the block statement maintains its scope and does not pollute the outer scope of the function.

### Const

In the previous examples, there is a lot of data mutation of defined variables within their scope. With `const`, things get a little more concrete. When defining a variable as a `const` the compiler will not allow the use of that variable again or allow a change to its value or primitive type.

To see this in action, the following example illustrates the variable of `i` and then reassigning its value to `2` and then logging that value. The next block of code uses the `const` keyword. Trying and change that variable's value the JavaScript engine will return a `type` error.

```js
let i = 1;
i = 2;
console.log(i); // 2

const e = 1;
e = 2;
console.log(e); // "TypeError: Assignment to constant variable.
```

It is important to remember that while you can't directly change the type and value of a `const` assignment, with an array or object, its content can be modified. It's common practice to define arrays and objects as `const` variables for this reason.

In the following example the variable of `foo` is an array. The next line of code adds new elements to that array. This does not violate the use of `const`.

```js
const foo = [1, 2, 3];

foo.push(4);

console.log(foo); // [1, 2, 3, 4]
```

Keep in mind, this is NOT the same as reassigning the total value of the array. For example, the following code will return an error.

```js
const foo = [1, 2, 3];

foo = ['a', 'b', 'c']

console.log(foo); // TypeError: Assignment to constant variable.
```

But this will work.

```js
const foo = [1, 2, 3];

foo[0] = 'a'
foo[1] = 'b'
foo[2] = 'c'

console.log(foo); // ["a", "b", "c"]
```

### Conclusion

As a common practice, most developers will use the `const` keyword when creating a variable unless there is a specific reason to use `let`.
