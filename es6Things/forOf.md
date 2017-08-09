# for ... of

If you have spent any time in JavaScript, you have written `for()` loops, that is a guarantee. The JavaScript `for()` loop is one of the bedrocks of the language, it's your go-to solution right up there with building a function or consuming an array.

Just for reference, let's talk about a simple situation where we have an function that takes an array and runs the elements through a `for()` loop to print out a statement to the console. In the following example I have an array of names and a function that I can pass that into. The function itself will loop though the names in the array and then print out a string template to the console.


```js
var names = ['Tom', 'Dick', 'Harry', 'Alice', 'Peter'];

function printNames(args) {
  for (i = 0; i < args.length; i++) {
    console.log('Hi, my name is ' + args[i]);
  }
}

printNames(names);
```

Pretty standard stuff there. So, what's the deal with this `for ... of` thing then? What's interesting to me is, when I discovered this I was reminded of the `@each` loop in Sass. For example, say you have a list of colors and you need to output individual color based selectors? Yeah, this is a terrible example, but work with me here.

```scss
$colors: orange, blue, green, yellow;

@each $color in $colors {
  .#{$color}-selector {
    color: $color;
  }
}
```

Something about that looks very familiar? We have a list (array) and we have a loop that will iterate through that list and pluck out the singular item from the list and perform an action. The output would be the following.

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

Now, let's look at the `for ... in` loop. It's basically the same thing. You can easily perform a loop that will iterate through the items in an array and perform an action. In the following example, the concept is the same as in Sass, but the syntax is all JavaScript. We start with the typically `for()` syntax, but inside we no longer use the traditional loop construct. We don't care about setting the value of `i`, we don't care about setting the loop to be always aware of `i < args.length` and we don't care about our iteration progression.

All we care about is getting the single item out of the array. In most cases this is easily readable by using the pluralization of names. `color` of `colors`. `name` of `names`. Etc ...

Looking at our previous example we can change things up a little to use this new syntax and make the code simpler to read IMHO. Oh, and we can jazz things up a little with some ES6 syntax as well. First we have our array of `names`. Then in the `for()` loop we simply set the singular variable of `name` in `names`. That's it!

Inside the function we can use a new tool called [template literals](/es6Things/templateLiterals.html) which makes the development of templates strings much easier. And we can call in the value of a variable using the `${}` syntax, much like the `#{}` syntax used in the previous Sass example.

Unlike Sass, we need to still identify the value of the iterator in the loop. Much like in the previous JS example where we needed to have `args[i]` where `args` was the array and `[i]` is the step in the iterator, we can use `${args}` to grab the array object and then `[arg]` to get the iterator.


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


### What about objects?

The ability to loop through data is not restricted to arrays. Objects, of course, can be very complex, but very powerful as well. For example, let's imagine that we have an object that describes my car. And then we need a function that will look into that object and report out it's properties. In the following example we have the object of `myVehicle` and it has four properties.

```js
const myVehicle = {
  'make': 'Toyota',
  'model': 'Scion',
  'color': 'black',
  'year': 2008
}
```

This object is stored in a database somewhere, so we can't easily see this object as we do now. How can we easily find out how many properties, or `keys` are in this object? To answer that, let's start our function. In that function we will console log out a template literal that is using the `Object.keys()` method to determine the length of `keys` in the object.

```js
function loopObject(obj) {
  console.log(`Number of properties: ${Object.keys(obj).length}`);
}


loopObject(myVehicle); // "Number of properties: 4"
```

Good start, for the next step we can use the `for ... in` loop to iterate over the keys in an object and return a new string that illustrates the keys in this object. To start we need to crate the `str` variable so that we can add to that as we iterate over the loop.

Notice inside the `for()` loop we don't simply update the value of `str`, but we need to iteratively add to the value of `str` via the `+=` operator. If we simply used the `=` operator, then the last value in the loop would be the value of `str`. But the `+=` operator, which is short hand for `var = var + value` iteratively adds content to this string variable with each loop.

```js
function loopObject(arg) {
  console.log(`Number of properties: ${Object.keys(arg).length}`);

  let str = '';

  for (let key in arg) {
    str += `${key}, `
  }

  console.log(str);
}

loopObject(myVehicle); // "make, model, color, year, "
```

Making great progress here. As of now we know how many properties are in the object and we know what the keys are. The last step is to list our the values of each key in this string. This is actually pretty easy, to do this we need to get the iterative value of the object key. Remember, `items[item]`.

```js
function loopObject(arg) {
  console.log(`Number of properties: ${Object.keys(arg).length}`);

  let str = '';

  for (let key in arg) {
    str += `${key}: ${arg[key]}, `
  }

  console.log(str);
}

loopObject(myVehicle); // "make: Toyota, model: Scion, color: black, year: 2008, "
```

## Conclusion

To wrap this up, you can see how the new `for ... in` loop is just as powerful and useful as the traditional `for()` loop, but simply reads better. Having written countless `@each` loops in Sass, this one really just jumps out at me and I am in the process of removing manual iterators from my life.
