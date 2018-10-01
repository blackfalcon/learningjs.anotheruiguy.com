> {{book.title}} v{{book.version}}

# The bind() method

As we have seen in previous examples, the concept of `this` can be confusing as it's a floating state where its value is always within context. If you are not 100% solid on how `this` works up to now, please go back and review the previous examples before moving forward.

The next example of using `this` gets really abstract with the `bind()` function. Using the `.bind()` function, we are able to bind data objects to a function at will. This is a big deal as this can drastically change the outcome of the function you are working with.

As we know, we can create an object, and inside that object we can assign a function to a property, making this a method within the object. So, let's do that. In the following example, I will create a new object for `doorbell` that will contain its sound and a method to log that sound to the console.

```js
const doorbell = {
  sound: 'ring',
  logSound: function() {
    console.log(this.sound);
  }
}
```

By calling that object with its method we will get the sound logged to the console.

```js
doorbell.logSound(); // `ring`
```

This is all pretty straightforward at this point. `this.sound` is within the context of the object, so there is direct awareness of the value we are looking for.

Let's get a little crazy here. In JavaScript, we can assign a function to the value of a variable. Listen to what I am saying here, we can assign the function itself as the value of a variable, not just the output of the function. This makes a YUUGE difference.

For example, if we were to create a new variable, `doorbellSound` and assign the method so that it returns its value, we will get `ring`, as this is the return of the function and is assigned to the variable, not the function itself.

```js
let doorbellSound = doorbell.logSound();
doorbellSound; // `ring`
```

But what if we only assign the method to the variable so that we can run that variable as a function?

```js
let doorbellSound = doorbell.logSound;
doorbellSound(); // undefined
```

This doesn't work!? Why? The reason really is, when we are running the method within the context of `doorbellSound()`, `this` is totally lost. Think about it, when we are calling `doorbellSound()` we are calling an object method without context. One way to fix this is to set a global variable and then `this` will have some value in context. But ... do we really want global vars?

```js
var sound = 'bing'

const doorbell = {
  sound: 'ring',
  logSound: function() {
    console.log(this.sound);
  }
}

let doorbellSound = doorbell.logSound;
doorbellSound(); // "bing"
```

We know that when we assign an object's method to a variable and run that, we have lost context with the data we are looking for. So imagine that you can just reach out and grab some data context and shove it right back in? Well, you don't have to imagine as we can actually do this.

In the next example, I am redefining the `doorbellSound` variable to `.bind()` the `doorbell` data to this context.

```js
let doorbellSound = doorbell.logSound.bind(doorbell);
doorbellSound(); // `ring`
```

What has happened here? Setting a variable with the value of a method in the global scope without context, we get `undefined`. By applying the `.bind()` method, we can reset the context of the method back to itself and thus return the value we are looking for.

Also, note that this is all specific to how we are making our method calls. Remember, if we simply call the method to return the value back to the variable, there is no need for `.bind()`.

```js
let doorbellSound = doorbell.logSound();
doorbellSound; // `ring`
```

## But why ... ?

Ok, so this all sort of makes sense within this example, but why would I want to use this like this versus just accessing the object and its method directly? That's a tough question to answer, so let's imagine a new scenario. Let's imagine that we have two objects, one for `doorbell` and one for `oldCar`. In each object, we have a value for the `sound` that this object makes.

```js
const doorbell = {
  sound: 'ring'
}

const oldCar = {
  sound: 'AHHOOOGAH!!'
}
```

Then, to abstract things, let's create another object that contains the method that we can use to log the sound.

```js
const thing = {
  logSound: function() {
    console.log(this.sound);
  }
}
```

Now we have three objects, two that are specific and have value and one that is abstract and only contains a method. Can you see what's happening next? It's at this time we can create a new variable that will reference the `thing` object that has the method of `logSound` and then `.bind()` the data for `oldCar`. Holy crap ...

```js
let newThing = thing.logSound.bind(oldCar);
newThing(); // "AHHOOOGAH!!"
```

And what about `doorbell`?

```js
let newThing = thing.logSound.bind(oldCar);
let anotherNewThing = thing.logSound.bind(doorbell);

newThing(); // "AHHOOOGAH!!"
anotherNewThing(); // "ring"
```

And there you have it. If you were able to get your head wrapped around this, then give yourself a High Five! This is a tough one and its uses are vast and interesting.

## Wait ... what about .call()?

If at this point you are asking yourself, "_Isn't this much like the `.call()` method?_" you would be right. Remembering back to the `.call()` method we had the following example data and function for our Mad Libs.

```js
const madLibsOne = {
  adj: 'slimy',
  verb: 'wiggle',
  noun: 'Texas',
}

const madLibsTwo = {
  adj: 'purple',
  verb: 'rain',
  noun: 'dove',
}

function printMadLib(nounOne, nounTwo) {
  console.log(`Our school cafeteria has really ${this.adj} food. Just thinking about it makes my stomach ${this.verb}. The hamburgers tastes like ${this.noun}. My friend Dave likes the ${nounOne} and thinks that it's made of ${nounTwo}.`)
}
```

Using the `.call()` method we were able to take the function, use the method, pass in the object and satisfy the arguments and produce a result. Using the `.bind()` method we can bind this data to a new function. The thing to remember is that `.call()` will use the function, accept the object for `this` and then using the arguments returns a result.

With `.bind()` on the other had, you can do the same thing, but what is returned is not a result, but another function that is bound to the object.

For example, we can create a new `newLib` variable that will pass the `printMadLib()` function, and in this process we will bind the `madLibsOne` object to it.

Now, `newLib()` contains all the features of the `printMadLib()` function, plus the data of the `madLibsOne` object. As a last step, we can pass in the last two arguments as you would the `printMadLib()` function and get a result.

```js
let newLib = printMadLib.bind(madLibsOne);

newLib('cat', 'dog'); // "Our school cafeteria has really slimy food. Just thinking about it makes my stomach wiggle. The hamburgers tastes like Texas. My friend Dave likes the cat and thinks that it's made of dog."
```

So, both methods are very similar, but differ based on how you want to use your functions and data.
