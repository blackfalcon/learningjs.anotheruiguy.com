> {{book.title}} v{{book.version}}

# Using the filter() and map() methods

By now you should have a pretty clear understanding about how the new `filter()` and `map()` methods work and their quick ability to iterate over an array and output a new array without additional effort. And in most cases, the ability to filter data and transform data with one line of code.

Having said that, let's imagine a few more scenarios where we can use the `filter()` and `map()` methods together for the desired output.

## Filtering and mapping an array

In this scenario, we will have an array of random numbers and the functionality that we are looking for is; filter out all the numbers that have a modulo of `0` and then on each number in the new array, map the function of `Math.sqrt()`. Finally, return that new array.

To start out, let's create our array of numbers.

```js
const numbers = [6374, 77, 964, 235, 17, 33, 67, 18]
```

Using the `filter()` method on this array is pretty straightforward. The function that we will pass it will use `el` to iterate on the elements in the array and the condition we will set is if `el % 2 === 0`. Again, no need to create a loop or an `if()` statement to address the condition.

```js
const evenNumbers = numbers.filter(el => el % 2 === 0)
```

Last we need our `map()` method to apply a function to each element in the array. Again using `el` as the iterator argument and then passing the value of `el` into the `Math.sqrt()` function.

```js
const sqrt = evenNumbers.map(el => Math.sqrt(el))
```

For the last step, we can run `console.log(sqrt)` for our result.

```js
[79.8373346248483, 31.04834939252005, 4.242640687119285, 6.6332495807108, 23.706539182259394]
```

## Filtering and mapping an array of objects

In this scenario, we will have an array of objects that represent characters from Star Wars. The functionality we are looking for is to enter a character's name and then output how many films that character appears in and what those films are. All of this without any `for()` loops.

For starters, our data.

```js
const characters = [
  {
    name: 'Han Solo',
    appearance: 'Star Wars'
  }, {
    name: 'Han Solo',
    appearance: 'Empire Strikes Back'
  }, {
    name: 'Han Solo',
    appearance: 'Return of the Jedi'
  }, {
    name: 'Han Solo',
    appearance: 'The Force Awakens'
  }, {
    name: 'Jabba the Hutt',
    appearance: 'Star Wars'
  }, {
    name: 'Jabba the Hutt',
    appearance: 'Return of the Jedi'
  }, {
    name: 'TK-421',
    appearance: 'Star Wars'
  }, {
    name: 'BB-8',
    appearance: 'The Force Awakens'
  }, {
    name: 'BB-8',
    appearance: 'The Last Jedi'
  }, {
    name: 'Jabba the Hutt',
    appearance: 'Phantom Menance'
  }
]
```

For our function, we want two parameters. The `name` of the character and the `object` data we will use.

```js
function appearances(name, obj) {
  ...
}
```

To start we will use a `filter()` method on the array of objects. If we return the value of that variable we should see a filtered down list of objects.

```js
function appearances(name, obj) {
  const myFilter = obj.filter(el => el.name === name);

  return myFilter;
}
```

For example, if we now run `console.log(appearances('Jabba the Hutt', characters));` we will see the following output.

```js
[[object Object] {
  appearance: "Star Wars",
  name: "Jabba the Hutt"
}, [object Object] {
  appearance: "Return of the Jedi",
  name: "Jabba the Hutt"
}, [object Object] {
  appearance: "Phantom Menace",
  name: "Jabba the Hutt"
}]
```

Great start. For the next part of this functionality, we need to get an array of films that this character appeared in. In the following updated code, I will take the new array of objects and then use the `map()` method on that array to return of all the `appearance` values.

```js
function appearances(name, obj) {
  const myFilter = obj.filter(el => el.name === name);
  const appearArray = myFilter.map(el => el.appearance);

  return appearArray;
}
```

Again, running `console.log(appearances('Jabba the Hutt', characters));` I will get the following return.

```js
["Star Wars", "Return of the Jedi", "Phantom Menace"]
```

At this point, we know how to filter down an array of objects to a nice subset and store that in memory. Next, we can map that return and retrieve a property of each object and return that as an array. The next part of the functionality is to return the name of the character. There are a couple of ways we can do this, but in this example, I want to quickly illustrate how you could use the `find()` method. This is much like the `filter()` method, but instead of returning all the found elements, it only returns the first one.

The following line of code is exactly like the `filter()` method line, except it's only going to return a single object from the array.

```js
const myFilter = obj.find(el => el.name === name);
```

This line is so similar, we would be remiss if we didn't store this in a function expression.

```js
const filterFunction = el => el.name === name;
```

Now we can update our `myFilter()` and `character()` functions to use this expression instead of writing it in-line.

```js
const myFilter = obj.filter(filterFunction);
const character = myFilter.find(filterFunction)
```

To complete this function, using properties of the `myFilter` array, I can get the length of the array, and as an added bonus, using the number of objects in the array, I can determine if the word `film` should be plural or not.

Last, using all this data, I can create a string for output.

```js
function appearances(name, obj) {
  const filterFunction = el => el.name === name;

  const myFilter = obj.filter(filterFunction);
  const appearArray = myFilter.map(el => el.appearance);
  const character = myFilter.find(filterFunction)

  const apperances = myFilter.length;
  const plural = apperances <= 1 ? 'film' : 'films';
  const str = `${character.name} appears in ${apperances} ${plural}. ${appearArray.join(', ')}`;

  return str;
}
```

Running `console.log(appearances('TK-421', characters));` I get ...

```js
"TK-421 appears in 1 film. Star Wars"
```

Running `console.log(appearances('BB-8', characters));` I get ...

```js
"BB-8 appears in 2 films. The Force Awakens, The Last Jedi"
```

At this point, you should be able to edit the data and output more strings based on any condition.
