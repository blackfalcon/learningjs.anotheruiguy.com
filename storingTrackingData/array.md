> {{book.title}} v{{book.version}}

# Arrays

When learning about variables, their different declaration keywords, and primitive types, the first big step into learning complex data is the **array**.

> The JavaScript **Array** object is a global object that is used in the construction of arrays; which are high-level, list-like objects.

In JavaScript, an array is an object where you store data in a list-like format, versus that of key/value pairs in an object, that will be discussed later. Anything can be stored in an array, primitive type values, objects, functions and even other arrays.

The following code illustrates a few different arrays.

```js
const numberArray = [1, 2, 3, 4];
const stringArray = ['Tom', 'Dick', 'Harry'];
const anythingGoesArray = [1, 'Tom', '2', 'Dick', false, getSchoolData(), numberArray, stringArray];
```

If it's data, it can be stored in an array. What makes an array interesting is that the data is in an indexed collection. This gives developers some super powers for how they can consume, iterate over and manage data.

## Array indexing simplified

As an example, here is an array of cities that a band has toured and then an array of cities that the band still has scheduled.

```js
const citiesToured = ['Milwaukee', 'Madison', 'Seattle', 'San Francisco'];
const citiesScheduled = ['Minneapolis', 'Chicago', 'New York', 'Boston', 'Los Angles'];
```

Each item in each array has an index, the arrays could also be visualized like this. It is important to remember that an array index starts at `0`.

```js
const citiesToured = ['Milwaukee'[0], 'Madison'[1], 'Seattle'[2], San Francisco'[3]];
const citiesScheduled = ['Minneapolis'[0], 'Chicago'[1], 'New York'[2], 'Boston'[3], 'Los Angles'[4]];
```

Knowing this, the following action can be performed on each array.

```js
// find value at specific index
var firstCity = citiesToured[0];
console.log(firstCity) // "Milwaukee"

// replace value at specific index
citiesScheduled[0] = 'St. Paul';
console.log(citiesScheduled) // ["St. Paul", "Chicago", "New York", "Boston", "Los Angles"]
```

## Multidimensional arrays

Arrays can go from being very simplistic to becoming extremely complex in a blink of an eye. Not only can arrays contain a list of data in multiple primitive formats, but an array can also be a collection of arrays.

Take a calendar app for example. The app requires data to be in an array and this array is required to have an array of days and an array of months. It would look something like this.

```js
const calendar = [];
calendar[0] = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
calendar[1] = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sept', 'Oct', 'Nov', 'Dec'];

console.log(calendar); // [["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"], ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sept", "Oct", "Nov", "Dec"]]
```

Multidimensional arrays are no different really than standard arrays. For example, an array in the array is indexed the same way that the elements in the arrays are. The following code illustrates how to create new variables by specifying the indexes within the `calendar` array to find a day of the week and a month.

```js
const day = calendar[0][1];
const month = calendar[1][4];
console.log(`It's a ${day} in ${month}`); // "It's a Tuesday in May"
```

## Array properties and methods

This is where things get really exciting. The Array object has a series of methods that allow for a long list of actions to be performed on an array. To start, here are some new arrays that will be used later.

```js
const percussion = ['Steelpan', 'Triangle', 'Xylophone'];
const stringed = ['Banjo', 'Guitar', 'Violin'];
const wind = ['Bassoon', 'Flute', 'Oboe'];
```

The following examples are some commonly used Array tools. For a really in-depth listing of Array properties and methods, see the [Array docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) on MDN.

To see a [running version](https://jsbin.com/qajepiv/edit?js,console) of the following examples.

#### Array.length

Array.length is a property of the Array Object and will return an integer of elements in an array

```js
let listLength = percussion.length;
console.log(listLength); // 3
```

#### Array.prototype.push()

A method that adds one or more elements to the end of an array. Most commonly used in a loop.

```js
percussion.push('Bass drum');
console.log(percussion) // ["Steelpan", "Triangle", "Xylophone", "Bass drum"]
```

#### Array.prototype.unshift()

A method that adds one or more elements to the front of an array.

```js
wind.unshift('Pan flute', 'Piccolo');
console.log(wind); // ["Pan flute", "Piccolo", "Bassoon", "Flute", "Oboe"]
```

#### Array.prototype.concat()

A method used to merge one or more arrays into the starting array.

```js
let instruments = percussion.concat(stringed, wind);
console.log(instruments); // ["Steelpan", "Triangle", "Xylophone", "Bass drum", "Banjo", "Guitar", "Violin", "Pan flute", "Piccolo", "Bassoon", "Flute", "Oboe"]
```

#### Array.prototype.includes()

A method that determines whether an array includes a certain element, returning a boolean.

```js
let isGuitar = stringed.includes('Bassoon');
let isWind = wind.includes('Oboe');
console.log(isGuitar); // false
console.log(isWind); // true
```

#### Array.prototype.pop()

A method that removes the last element from an array and returns that element. This method mutates the length of the array.

```js
wind.pop();
console.log(wind, wind.length); // ["Pan flute", "Piccolo", "Bassoon", "Flute"], 4
```

#### Array.prototype.shift()

A method that removes the first element from an array and returns that element. This method changes the length of the array.

```js
stringed.shift();
console.log(stringed, stringed.length); // ["Guitar", "Violin"], 2
```

#### Array.prototype.slice()

A method that returns a shallow copy of a portion of an array into a new array object selected from beginning to end **(end not included)**. The original array will not be modified.

```js
let slicedInstruments = instruments.slice(3, 6);
console.log(slicedInstruments); // ["Bass drum", "Banjo", "Guitar"]
```

#### Array.prototype.toString() & Array.prototype.join()

Both the `toString()` and `join()` methods return an array back in the form of a string. The difference being that `join()` allows for a specification of character whereas `toString()` returns the array in a string with commas.

```js
let percussionStr = percussion.join(', ');
let stringedStr = stringed.toString();

console.log(percussionStr); // "Steelpan, Triangle, Xylophone, Bass drum"
console.log(stringedStr); // "Guitar,Violin"
```

#### Array.prototype.sort()

A method that sorts the elements of an array in place and returns the array.

```js
let sortedInstruments = instruments.sort();
console.log(sortedInstruments); // ["Banjo", "Bass drum", "Bassoon", "Flute", "Guitar", "Oboe", "Pan flute", "Piccolo", "Steelpan", "Triangle", "Violin", "Xylophone"]
```

#### Array.prototype.splice()

A method that changes the contents of an array by removing existing elements and/or adding new elements.

```js
console.log(percussion); // ["Steelpan", "Triangle", "Xylophone", "Bass drum"]
percussion.splice(2, 1, 'Bongo', 'Symbol');
console.log(percussion); // ["Steelpan", "Triangle", "Bongo", "Symbol", "Bass drum"]
```

#### Array.prototype.reverse()

A method that reverses an array in place.

```js
let percussionRev = percussion.reverse();
console.log(percussionRev); // ["Bass drum", "Symbol", "Bongo", "Triangle", "Steelpan"]
```

## Fun with Arrays

Having gone through a series of examples that show the different methods that can be performed on arrays, the following example illustrates how to build a random string generator.

#### Random string generator

In this example, there is a function called `randomInst()` that has a single parameter `arr`. There are two Array methods used in this function, `.length()` to determine the max value of a randomly generated number that can be passed into the `[]` and retrieve the element at that index of the array.

The value for the variable of `myBand` calls the `randomInst()` function and passes in each of the three arrays being used.

With each execution of this script, the function will create a new randomly generated sentence.

```js
function randomInst(arr) {
   const number = Math.floor(Math.random() * arr.length);
  return arr[number];
}

const myBand = `In my band, I have a ${randomInst(percussion)} on percussion, a ${randomInst(stringed)} on strings, and a ${randomInst(wind)} blowing sweet wind!`
console.log(myBand); // "In my band, I have a Bass drum on percussion, a Violin on strings, and a Piccolo blowing sweet wind!"
```

#### Array, string generator in a loop

The following code illustrates how JavaScript can iterate over the elements in an array using a `for ... in` loop. With each iteration of an element, the loop will log a new string to the console. The `for ... in` loop will be discussed further in-depth later in this course.

```js
for (let instr in percussionRev) {
  let newStr = `I will play the ${percussionRev[instr]}!`;
  console.log(newStr);
}

// "I will play the Bass drum!"
// "I will play the Symbol!"
// "I will play the Bongo!"
// "I will play the Triangle!"
// "I will play the Steelpan!"
```
