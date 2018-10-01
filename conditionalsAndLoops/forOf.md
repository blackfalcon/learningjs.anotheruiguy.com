> {{book.title}} v{{book.version}}

# for ... of

To understand the `for ... of()` loop, we need to understand what makes it different from that of the `for ... in()` loop. The `for ... in()` loop will only loop over enumerable properties whereas the `for ... of()` loop will loop over iterable objects. These are objects like Array, Map, Set, and String.

Notice in that list is NOT an Object.prototype. This is a very important distinction to remember. For the most part, `for ... in()` and `for ... of()` loops will to the same thing with similar syntax.

For example, let's say we have a string that we need to loop over.

```js
const str = "Javascript is fun!";
```

We can use a `for()` loop. Notice in the example how we need to reference the array and the index in order to get the value, `str[i]`.

```js
for (let i = 0; i < str.length; i++) {
  console.log(str[i]);
}
```

Using the `for ... in()` loop, we again need to point to the array and its index in order to get the value.

```js
for (let i in str) {
  console.log(str[i])
}
```

With the `for ... of()` loop, the key difference is that the loop is not looking for the index of enumerable properties, but simply the iterable objects within. Notice in the following example, I no longer need to reference the array, but simply ask for the value of `i`.

```js
for (let i of str) {
  console.log(i)
}
```

This works exactly the same for iterating over an array of anything. Let's say that we have an array made of up a couple strings, a number, a function
and a variable.

```js
const alpha = 'ABCD'

function foo() {
  return 'bar'
}

const arr = ['Tom', 'Dick', 12, foo, alpha];
```

Again we can simply loop over the contents of the array.

```js
function printNamesFor(args) {
  for (i = 0; i < args.length; i++) {
    console.log(`The array element is ${typeof(args[i])}`)
  }
}
```

We could use the `for ... in()` loop much in the same way as the previous `for()` loop.

```js
function printNamesIn(args) {
  for (let arg in args) {
    console.log(`The array element is ${typeof(args[arg])}`);
  }
}
```

With the `for ... of()` loop, just like before, we simply need to reference `arg` value and not be worried with the index of `args`.

```js
function printNamesOf(args) {
  for (let arg of args) {
    console.log(`The array element is ${typeof(arg)}`);
  }
}
```

When it comes to objects, the `for ... of()` loop is not your tool. The keys are not iterable, but are enumerable and you will get a TypeError like the following.

```js
"TypeError: obj is not iterable
```
