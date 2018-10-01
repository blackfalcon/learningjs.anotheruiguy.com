> {{book.title}} v{{book.version}}

# for ... in

If you have spent any time in JavaScript, you have written `for()` loops, that is a guarantee.

For example, we have a function that takes an array and runs the elements through a `for()` loop to print out a statement to the console. In the following example, I have an array of names and a function that I can pass that into. The function itself will loop through the names in the array and then print out a string template to the console.


```js
var names = ['Tom', 'Dick', 'Harry', 'Alice', 'Peter'];

function printNames(args) {
  for (i = 0; i < args.length; i++) {
    console.log('Hi, my name is ' + args[i]);
  }
}

printNames(names);
```

So why the `for ... in` then? When I discovered this I was reminded of the `@each` loop in Sass. For example, you have a list of colors and you need to output individual color based selectors?

```scss
$colors: orange, blue, green, yellow;

@each $color in $colors {
  .#{$color}-selector {
    color: $color;
  }
}
```

We have a Sass list (which is basically an array) and we have a loop that will iterate through that list and pluck out the singular item from the list and perform an action. The output would be the following.

```css
.orange-selector {
  color: orange;
}

.blue-selector {
  color: blue;
}

.green-selector {
  color: green;
}

.yellow-selector {
  color: yellow;
}
```

The `for ... in` loop is very similar.

> The `for ... in` statement iterates over the enumerable properties of an object. For each distinct property, statements can be executed.

Interestingly enough, this is not a new ES6 feature of JavaScript, this has actually been around since ES1. I find the syntax very easy to follow.

```js
for (let variable in object) {
  ...
}
```

Much like a standard `for()` loop, we need to set the initial variable, but there is no need to set it's initial value, `let variable` is all we need.

Next, we need to object that we will iterate over, `in object`. For all intents and purposes, we will be using an Array, not a literal object.

In the following example, the concept is similar as in Sass, but the syntax is all JavaScript. Start with the typical `for()` syntax, but inside the parens, there is no need for the loop construct. No need for the `i`, no need to be aware of `i < args.length` and we don't care about our iteration progression `i++`. What we care about is iterating over a single item within the array. In most cases, this is easily readable by using the pluralization of names, e.g. `color` of `colors` or `name` of `names`.

Looking at our previous `for()` loop example we can change things up a little to use this new syntax and make the code simpler to read. Using an array of `names`, in the `for()` loop we simply set the singular variable of `name` in `names`.

Similar to the `for()` loop we need to still identify the value of the iterator in the loop. Where we needed to have `args[i]` where `args` was the array and `[i]` is the index of the iterator, we can use `${args}` to grab the array object and then `[arg]` to get the iterator.


```js
const names = ['Tom', 'Dick', 'Harry', 'Alice', 'Peter'];

function printNames(args) {
  for (let arg in args) {
    console.log(`Hi, my name is ${args[arg]}`);
  }
}

printNames(names)
```

And there you have it. The output would look like the following.

```js
"Hi, my name is Tom"
"Hi, my name is Dick"
"Hi, my name is Harry"
"Hi, my name is Alice"
"Hi, my name is Peter"
```


## The for ... in loop and an object

Let's imagine that we have an object that describes a car and we need a function that will look into that object and report out its properties.

```js
const myVehicle = {
  make: 'Toyota',
  model: 'Scion',
  color: 'black',
  year: 2008
}
```

To iterate over an object, the `Object.keys()` method is used. In the following example function, I am simply passing in the object as an argument and printing out the `Object.keys().length` as you would with an array.

```js
function loopObject(obj) {
  console.log(`Number of properties: ${Object.keys(obj).length}`);
}

loopObject(myVehicle); // "Number of properties: 4"
```

Next the `for ... in` loop is to iterate over the keys in the object and return a new string that prints the keys of this object to the console.

Inside the `for .. in` loop, with each iteration over the object we can simply push the value of `key` into a new array.

```js
function loopObject(obj) {
  console.log(`Number of properties: ${Object.keys(arg).length}`);

  let arr = [];

  for (let key in obj) {
    arr.push(key)
  }

  console.log(arr);
}

loopObject(myVehicle); // ["make", "model", "color", "year"]
```

We know how many properties are in the object and we know what the keys are. To see what the values are inside the object we simply update the value of that is being pushed to the array from the `key` itself, but to the key's value within the `obj`, `arg[key]`

```js
function loopObject(obj) {
  console.log(`Number of properties: ${Object.keys(obj).length}`);

  let arr = [];

  for (let key in obj) {
    arr.push(obj[key])
  }

  console.log(arr);
}

loopObject(myVehicle); // ["Toyota", "Scion", "black", 2008]
```

### Enumerable and non-enumerable object properties

It should be noted that when working with Objects and the `for ... in` loop, you need to be aware of the existence of non-enumerable properties as these will not show up in the looped output. Properties in a JavaScript object are set to `Enumerable[true]` by default, but using the `Object.defineProperty()` method, you can create a new property and `Enumerable[false]` by default.

Looking back at the `myVehicle` object example, let's imagine that we are going to set a secret property.

```js
Object.defineProperty(myVehicle, 'secret', {
  value: 'involved in accident'
});
```

When we run our `loopObject()` function, we still get the same return as we did prior to setting the new secret property.

```js
loopObject(myVehicle); // ["make", "model", "color", "year"]
```

Although this property does not show up using the `for ... in` loop, we can still directly access the `secret` value.

```js
console.log(myVehicle.secret); // "involved in accident"
```

How do we know if a property is enumerable or not? Well, the first function basically tells us what properties are enumerable by simply returning the keys. At this point though we do not know all the properties of the object, to see all the properties of an object, regardless of a property's enumerable state, we can use the `Object.getOwnPropertyNames()` method.

```js
console.log(Object.getOwnPropertyNames(myVehicle)) // ["make", "model", "color", "year", "secret"]
```

Now that we have a full list of all the object's properties, we can use the `propertyIsEnumerable()` prototype method on the object itself.

```js
console.log(myVehicle.propertyIsEnumerable('secret')); // false
```

You may be asking yourself as to why you would use this? The leading reason, as illustrated in this example, you may have data that you do not want to be exposed with any call on that object. As long as the API is known about the objects in your app you can develop special cases where that data can be exposed, for example, seeing information about a car's accident history.

## Conclusion

It is important to note that that the `for ... in` loop should not be used to iterate over an array where the index order is important. The order of iteration is implementation-dependent and there is no guarantee that iterating over an array will produce results in a consistent order.

Depending on the needs of your function, it may be better to use a standard `for()` loop with a numeric index or the `for ... of()` loop when iterating over arrays where the order of access is important.

