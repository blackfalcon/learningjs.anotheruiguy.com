> {{book.title}} v{{book.version}}

# forEach(); it's a loop, it's a method

To set the stage, `forEach()` is a new ES6 method on the Array prototype, so don't try using this method on objects. The `forEach()` method really fits well in the Functional Programming paradigm versus the more imperative, or procedural, style.

An example of the `forEach()` method is where you need to iterate over the elements of an array and perform an action on each element.

The mandatory syntax for the `forEach()` method is the following.

```js
arr.forEach(function callback() {
  //your iterator
};
```

There is an optional `thisArg` value to use as `this` when executing the callback.

Within the function callback, you may pass in `currentValue, index, array` as arguments.

For our example, let's start out with some simple data.

```js
const obj = {
  name: 'Object',
  color: 'orange',
  amount: 100,
  isAwesome: true
}

const alpha = 'ABCD'

function foo() {
  return 'bar'
}

const myArr = ['Tom', false, 12, , '12', foo, alpha, obj];
```

With this data, there are easily two different ways that the `forEach()` method can be used. But just for reference, here is an example of how this would be done using a `for()` loop.

```js
function whatIsType(arg) {
  for (let i = 0; i < arg.length; i++) {
    console.log(`myArr[${i}] = ${typeof(arg[i])}`)
  }
}
```

What is happening here should be easily understandable. But again, using a `for()` loop here is very procedural. For something more imperative the `forEach()` method on the array comes is preferred because of the clear understanding of how this method operates.

One way we can use the `forEach()` method is inside another function and creating an anonymous callback function. In this example, the `whatIsType()` function contains a single parameter for the array. The `forEach()` method itself, I am passing down the arguments for `element` and `index`.

```js
function whatIsType(arg) {
  arg.forEach(function(element, index) {
    console.log(`array index [${index}] = ${typeof(element)}`)
  })
}

whatIsType(myArr);
```

This works, but there is little additional value there outside using another loop type. Where the `forEach()` loops really gain value is to simply append this method to an array and pass to the function you need to use. Let's refactor this function so that it is reusable and then use it on the array versus passing it into the array.

This time I will put the `element` and `index` arguments on the `whatIsType` function itself.

```js
function whatIsType(element, index) {
  console.log(`array index[${index}] = ${typeof(element)}`);
}
```

To use this function, I simply reference the array I want to use, append the `forEach()` method and then pass in the function I want to be used on the array. Of course, we can also bind this return to a variable for reference later.

```js
function whatIsType(element, index) {
  console.log(`array index[${index}] = ${typeof(element)}`);
}

const arrayTypes = myArr.forEach(whatIsType);
```
