# Invoked via a .bind() method

[jsBin](https://jsbin.com/kucihez/edit?js,console)

```js
// Utility function to keep things DRY and really screw with the concept of 'this'
function talking() {
  console.log(this.sound);
}

// Dog object
let dog = {
  sound: 'woof', // property of dog object
  talk: talking,
}

// Cat object
let cat = {
  sound: 'meow', // property of cat object
  talk: talking,
}

// Dog says 'woof'
dog.talk(); // using .talk method on dog object

// Set value of variable to that of the functon
let talkFunction = dog.talk;

// running function returns undefined as we have lost the scope of 'this'
talkFunction(); // undefined :(

// using .bind() to set the scope of this within this context
let boundTalkFunction = dog.talk.bind(dog); // woof

boundTalkFunction = dog.talk.bind(cat); // meow O_O

// running
boundTalkFunction();
```
