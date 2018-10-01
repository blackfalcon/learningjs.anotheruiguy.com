> {{book.title}} v{{book.version}}

# Destructuring arrays

The first object we will look at destructuring is the array. The following is a quick abstract example of how to destructure an array. First, we have some function that is returning whatever is passed into it. Then we have an array of many things.

```js
const foo = (arg) => {
  return arg;
}

const arr = [1, 'alpha', foo, 1, 2, false, 4, true];
```

To destructure this array we could, for example, take the first item and apply that value to a new variable of `num`. Then the second, bind to the variable of `str`. And last, bind `foo` to `funct`.

```js
const [num, str, funct] = arr;
```

Having done that we can now console log out the values of these new variables.

```js
console.log(num); // 1
console.log(str); // "alpha"
console.log(funct('yay')); // "yay"
```

But what of the remainder of the array? With the destructuring syntax, we can actually grab the remaining items in that array via the spread `...` syntax and simply place that into another array.

```js
const [num, str, funct, ...leftOvers] = arr;

console.log(leftOvers); // [1, 2, 3, 4, 5]
```

For another example of this, let's imagine that we are getting data into an app that is in an array and the need is to build a function that prints out a string that describes to contents of the array. The following example is how we could easily parse the data and print our statement.

```js
const listing1040Willow = ['remodeled kitchen', '4 bedrooms', '2.5 baths'];

const printListing = (arr) => {
  return `New listing with ${arr[0]}, ${arr[1]} and ${arr[2]}.`
}

console.log(printListing(listing1040Willow)) // "New listing with remodeled kitchen, 4 bedrooms and 2.5 baths."
```

Update! There is an update to the spec that the same data needs to be used in another function that needs to only print out part of the data, the bedrooms. We could simply reference the second element in the array and print that out.

```js
const printBedrooms = (arr) => {
  return `New listing with ${arr[1]}.`
}
```

This is all starting to sound very abstract. I'd prefer that we take the items from the array that we need and bind them to variables that we easily reuse.

```js
const feature = listing1040Willow[0];
const bedrooms = listing1040Willow[1];
const baths = listing1040Willow[2];

const printListing = () => {
  return `New listing with ${feature}, ${bedrooms} and ${baths}.`
}
const printBedrooms = () => {
  return `New listing with ${bedrooms}.`
}
```

In this example, this is pretty manageable. But let's agree that if we get a large amount of data and need to map that to a long list of variable names, this can get really tedious.

Destructuring allows us to quickly and easily unpack the values from an array and assign them to variables that can be used in our app.

```js
const listing1040Willow = ['remodeled kitchen', '4 bedrooms', '2.5 baths'];
const [feature, bedrooms, baths] = listing1040Willow;

const printListing = () => {
  return `New listing with ${feature}, ${bedrooms} and ${baths}.`
}
const printBedrooms = () => {
  return `New listing with ${bedrooms}.`
}

console.log(printListing()) // "New listing with remodeled kitchen, 4 bedrooms and 2.5 baths."
console.log(printBedrooms()) // "New listing with 4 bedrooms."
```
