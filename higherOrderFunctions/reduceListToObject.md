> {{book.title}} v{{book.version}}

# Reduce array to object

The `reduce()` method is packed full of uses. Like stated in the last section, it truly is the Swiss Army Knife of JavaScript.

In this example, imagine building an app that will take votes for a classroom special treat. The app will allow the children to enter anything they want, and it's our job to take that data and return a clear winner.

To start , we will imagine that the children have all had a chance to enter a vote. Aside from spelling mistakes, there will be some messy data in here that we will need to contend with as well.

```js
const votes = [
  'cookies', 'Cookies', 'chocolate', 'cup cakes', 'ice cream', 'candy', 'CANDY', 'candy',
  'Ice cream', 'cookies', 'cup cakes', 'Chocolate',  'cup cakes', 'candy', 'Candy',
  'Ice Cream', 'ice cream', 'cup cakes', 'candy', 'candy', 'Cup cakes'
]
```

There is a lot of data and to manually sort. This is where the `reduce()` function is very helpful.

The `reduce()` method takes two arguments, the `callback()` function and the `initialValue`. The `initialValue` does not need to be a number or a string, it can also be an empty object.

As illustrated in the following code, the `callback()` function is `tallyVotes()` and the `initialValue` is an empty object `{}`. What is left is to create the `tallyVotes()` callback function.

```js
const result = votes.reduce(tallyVotes, {})
```

For the `tallyVotes()` callback function, just like before, the `accumulator` and `currentValue` parameters need to be set. The `reduce()` method will automatically loop over the array, so there is no need to be concerned with that operation. All that is needed is the condition from which to return data.

In the `if()` statement, the condition is asking if the `accu[val]` exists. To address inconsistent capitalization `.toLowerCase()` is appended to the `val` argument. If `accu[val.toLowerCase()]` comes back as `true`, then `accu[val.toLowerCase()]` adds `1` to the existing property. If this does not exist, then `accu[val.toLowerCase()]` will create the object property and add `1`. The return is `accu`.

```js
const tallyVotes = (accu, val) => {
  accu[val.toLowerCase()] ? accu[val.toLowerCase()] += 1 : accu[val.toLowerCase()] = 1
  return accu
}
```

Just like the previous `reduce()` examples, this function is taking the initial value of an empty object `{}` and passing that onto the `accu` argument on the first pass. `val` on the other hand is maintaining a watch on all the elements in the array.

If this were to be logged out in the console, the first pass would show `cookies` is the first vote. This fails the condition and the new `cookies` property of the object will be created with the value of `1`.

```js
[object Object] { ... } // accu
"cookies" // val
```

On the next pass, `Cookies` is the next vote. The condition is meet as `cookies` already exists in the object, so the tally adds `1`.

```js
[object Object] { ... } // accu
"cookies" // val
[object Object] { cookies: 1 } // accu
"Cookies" // val
[object Object] { cookies: 2 } // accu
```

On the third pass, `chocolate` is the next vote. This fails the condition and the new object property is created with the value of `1`.

```js
[object Object] { ... } // accu
"cookies" // val
[object Object] { cookies: 1 } // accu
"Cookies" // val
[object Object] { cookies: 2 } // accu
"chocolate" // val
[object Object] { chocolate: 1, cookies: 2 } // accu
```

The `reduce()` method will continue on until all the elements in the array have been addressed. The final return of the `tallyVotes()` function is the final result of the `accu` argument.

```js
[object Object] {
  candy: 7,
  chocolate: 2,
  cookies: 3,
  cup cakes: 5,
  ice cream: 4
}
```

`candy` with `7` votes is the winner!

## Reduce the reducer

Reducing this large array down to an object is one thing, but what about reducing this object even further to output the winner? Keep in mind, `reduce()` is a method on the array prototype, so in order to do the following steps, the object's properties and values need to be distilled into some small arrays. Thankfully JavaScript has some methods to support this without additional coding.

From the object, it is required to find the highest vote tally. To quickly get all the values from the `results` object it is necessary to use the `Object.values()` method that will return an array of the object's enumerable properties. Here the `Object.values()` method will return the following array.

```js
[3, 2, 5, 4, 7]
```

From this array it is desired to return the highest number and assign that value to a variable called `topTally`.

For the reduce function, again using the `accu, val` parameters to iterate over, a procedural paradigm example would look like this.

```js
function reducer(accu, val) {
  let highValue = 0

  if(accu > val) {
    highValue = accu
  } else {
    highValue = val
  }

  return highValue
}
```

Leaning toward a more declarative approach, using the arrow syntax and a terse `if()` statement, the function can be boiled down to one line.

There is no need to set an initial value as this function will simply iterate over all the values in the array and look for the higher number.

```js
const topTally = Object.values(result).reduce((accu, val) => (accu > val) ? accu : val) // 7
```

At this point, it is known that the highest tally value in the `result` object is `7`. Next, it is needed to get back the property that this value is assigned to. This process is very similar to that addressed with the previous code, except this time using the `Object.keys()` method to return a list of all the keys in the object.

```js
["cookies", "chocolate", "cup cakes", "ice cream", "candy"]
```

The reducer is also very much like the previous one except with a slight difference. This time the code will not only iterate over the keys in the object, but their values as well, and depending on the high value, will return the key.

The following example illustrates a procedural solution to this problem.

```js
function reducer(accu, val) {
  let highVote = ''

  if(result[accu] > result[val]) {
    highVote = accu
  } else {
    highVote = val
  }

  return highVote
}
```

Being more declarative and terse once again, this same code can be written in one line.

```js
const topVote = Object.keys(result).reduce((accu, val) => result[accu] > result[val] ? accu : val ) // "candy"
```

Now that wthe `topVote` and `topTally` values from the `result` object are known, it is possible to produce the following string.

```js
console.log(`${topVote} wins with ${topTally} votes`) // "candy wins with 7 votes"
```

## Conclusion

The `reduce()` method is extremely versatile. Its use greatly reduces the dependency on lopping over arrays again and again. The example below illustrates the full solution to the problem using only the `reduce()` method to find and return the popular vote. At this point, you should be able to update the data and return an updated string that represents the data.

```js
const votes = [
  'cookies', 'Cookies', 'chocolate', 'cup cakes', 'ice cream', 'candy', 'CANDY', 'candy',
  'Ice cream', 'cookies', 'cup cakes', 'Chocolate',  'cup cakes', 'candy', 'Candy',
  'Ice Cream', 'ice cream', 'cup cakes', 'candy', 'candy', 'Cup cakes'
]

const tallyVotes = (accu, val) => {
  accu[val.toLowerCase()] ? accu[val.toLowerCase()] += 1 : accu[val.toLowerCase()] = 1
  return accu
}

const result = votes.reduce(tallyVotes, {})
const topVote = Object.keys(result).reduce((accu, val) => result[accu] > result[val] ? accu : val )
const topTally = Object.values(result).reduce((accu, val) => (accu > val) ? accu : val)

console.log(`${topVote} wins with ${topTally} votes`)
```
