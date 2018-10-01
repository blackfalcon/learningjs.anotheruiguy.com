> {{book.title}} v{{book.version}}

# The map() method

The new ES6 `.map()` method means what it implies, but its use is new and a little weird. The new `filter()` and `map()` methods are new ways to iterate over an array. So what is the `map()` method?

> The `map()` method creates a new array with the results of calling a provided function on every element in the calling array.

Much like the `filter()` method, `map()` is an automatic loop on an array that performs actions based on the function passed in. To get things started, let's look at a simple example. Let's imagine that you have an array of numbers and you need all the elements of that array converted to a string. It's easy to assume that you would see code similar to the following.

There is a new empty array, then a `for()` loop that iterates over each element in the array, updates it to a string and then pushes it to the empty array.

```js
const array = [4, 12, 555, 76];

const map = []

for (let i = 0; i < array.length; i++) {
  map.push(array[i].toString())
}

console.log(map) // ["4", "12", "555", "76"]
```

What if I told you that you could do this same work in a single line of code? Using the same array I can create a new variable `map` and the value of this is the result of a `map()` function on the array itself.

The function itself is pretty simple and we can easily use the arrow `=>` function syntax. Like the `filter()` function, we need a parameter, `el` in this example, which will maintain each iteration of an element in the array and perform the function statement on the other side of the arrow operator `el.toString()`. That's it. No need for an empty array to push to. Each iteration return will be applied to the new array being created by the `map()` method.

```js
const array = [4, 12, 555, 76]

const map = array.map(el => el.toString())

console.log(map) // ["4", "12", "555", "76"]
```

## Applying map() to objects in an array

To illustrate how `map()` can be used on an array of objects let's start with a simple object of people. Its properties are `name` and `home`.

```js
const myPeeps = [
  {
    name: 'Dale',
    home: 'Seattle'
  }, {
    name: 'Chris',
    home: 'Detroit'
  }, {
    name: 'Arron',
    home: 'Green Bay'
  }, {
    name: 'Alice',
    home: 'San Francisco'
  }, {
    name: 'Bruno',
    home: 'Montreal'
  }, {
    name: 'Kianosh',
    home: 'Boston'
  }
]
```

What we want to do with this array is take each property from each object and create a new string and then apply that to a new array. Of course, we could do this with a `for()` loop like before and push the return to a new array, but once again, let's perform this action with a single line of code.

Using the `map()` method on the `myPeeps` array of objects we use the `el` argument to iterate over each object in the array and get the `name` and `home` value. With each iteration of the `map()` the new string is returned to the `newArray` variable.

```js
const newArray = myPeeps.map(el => `${el.name} lives in ${el.home}`)

console.log(newArray)
// ["Dale lives in Seattle", "Chris lives in Detroit", "Arron lives in Green Bay", "Alice lives in San Francisco", "Bruno lives in Montreal", "Kianosh lives in Boston"]
```

What is extremely helpful with the new `map()` function, as well the `filter()` function, is that they are native methods on the array object. This results in not having to loop over an array and rebuild a new array. This means less memory, less code and less opportunity for bugs in your code.
