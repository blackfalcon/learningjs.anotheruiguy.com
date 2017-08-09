# Can you remove letters from a string?

This is an interesting question. I was asked something similar, but when reproducing this solution later, I decided to have a little more fun with it.

Let's say that the given question is this.

> You are considering a trip to Boston and you want to learn how to speak with a Boston accent. To do this, you need to build a function where you can pass in a string and remove all the `r`s. Once the `r`s are removed, put that string back together.

For example,

```
Where are the the cars?
```

will become,
```
Whee ae all the cas?
```


## Talk through the solution

To complete this solution, there are some basic concepts being used, that of [splitting](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) a string object into individual substrings using a specified string to determine the split.


```js
const myString = `We are putting our shorts in the car? Right you are.`;

function speakBoston(arr) {
  const stringArray = arr.split('');
  const stringLength = stringArray.length;
  const newStringArray = [];
  const alwaysBoston = `That's wicked awesome!`;
  let newString;
  let concatString;
  let i;

  for(i = 0; i < stringLength; i++) {
    const character = stringArray[i];

    if (character != 'r' && character != 'R') {
      newStringArray.push(character);
    }


  }
  newString = newStringArray.join('');
  concatString = `${newString} ${alwaysBoston}`;

  return concatString;
}

console.log(speakBoston(myString));
```
