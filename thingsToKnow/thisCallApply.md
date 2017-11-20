# Invoked via .call() or .apply() methods

It should be noted that while the syntax of these two functions is almost identical, the functional difference between the two is, the `call()` method accepts an argument list, whereas the `apply()` method accepts a single array of arguments.

## Quick intro to .call() and .apply()

Before we get too far into examples, let's briefly explore what the `.call()` and `.apply()` methods really do. As previously stated, the `.call()` and `.apply()` methods pretty much do the same thing, it's just that the `.call()` method operates on arguments and the `.apply()` method operates on argsArray. What does that mean? Here are the specs of both methods. What's consistent is the use of thisArg.

```js
// the call() method
function.call(thisArg, arg1, arg2, ...)

// the apply() method
func.apply(thisArg, [argsArray])
```

#### The call() method

The `.call()` method is defined as the following:

> The `call()` method calls a function with a given `this` value and arguments provided individually.

What does that mean? To put it simply, the `.call()` method will call a function that consists of a reference to `this`, but is void of context. Using this method we can set the scope of `this` to an object we want to use the values of, and then apply the arguments to the function. I like Mad Libs and they make really great examples, so let's imagine that we have a program that will allow you to create a simple Mad Lib that has two parts to do this. One is to use a pre-defined object of words, and the other is to supply two arguments for additional words.

Here are our pre-defined objects.

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
```

Then we have a function that will have the `this` value. This will print out our Mad Lib. In this function we see that there are references to `this` properties, but there is no context of `this` so these will return `undefined` if we try to run this function alone and simply add the two word arguments.

```js
function printMadLib(nounOne, nounTwo) {
  return `Our school cafeteria has really ${this.adj} food. Just thinking about it makes my stomach ${this.verb}. The hamburgers tastes like ${this.noun}. My friend Dave likes the ${nounOne} and thinks that it's made of ${nounTwo}.`
}


printMadLib('frog', 'rocks'); // "Our school cafeteria has really undefined food. Just thinking ..."
```

Using the `.call()` method we can use the same function, but reference the data from one of the pre-defined object and get a better return.

```js
printMadLib.call(madLibsOne, 'frog', 'rocks'); // "Our school cafeteria has really slimy food. Just thinking about it makes my stomach wiggle. The hamburgers tastes like Texas. My friend Dave likes the frog and thinks that it's made of rocks."

printMadLib.call(madLibsTwo, 'frog', 'rocks'); // "Our school cafeteria has really purple food. Just thinking about it makes my stomach rain. The hamburgers tastes like dove. My friend Dave likes the frog and thinks that it's made of rocks."
```

What this example illustrates is how we can execute a function, pass in the reference to the `this` object and then satisfy the arguments of the function for the desired output. The power this gives us is the ability to abstract functions/methods away from objects, reduce complexity and increase flexibility.

#### The apply() method

The `.apply()` method is defined as the following:

> The `apply()` method calls a function with a given `this` value, and arguments provided as an array (or an array-like object).

What does this mean and why is this different from `call()`? The syntax of this function is almost identical, but the main difference is that `call()` accepts an argument list, while `apply()` accepts an array. Let's refactor the previous example and instead of passing in the two additional nouns as individual arguments into the function, we can pack those into an array and then pass that array into the `.apply()` method along with the object reference.

```js
const thisArray = ['farmer', 'car'];
const thatArray = ['television', 'fireplace'];

printMadLib.apply(madLibsOne, thisArray);
printMadLib.apply(madLibsTwo, thatArray);
```

And the return we get in the console is the following:

```js
"Our school cafeteria has really slimy food. Just thinking about it makes my stomach wiggle. The hamburgers tastes like Texas. My friend Dave likes the farmer and thinks that it's made of car."
"Our school cafeteria has really purple food. Just thinking about it makes my stomach rain. The hamburgers tastes like dove. My friend Dave likes the television and thinks that it's made of fireplace."
```



## Further exploration of this

To explore these methods, let's take a look at the following example. Here I am creating two objects; the variable `fullname` and then the variable object of `obj`. Take a look at this code and then look at the outcome, the question is; how?

```js
var fullname = 'John Doe';

var obj = {
   fullname: 'Colin Ihrig',
   prop: {
      fullname: 'Aurelio De Rosa',
      getFullname: function() {
         return this.fullname;
      }
   }
};

console.log(obj.prop.getFullname()); // Aurelio De Rosa

var test = obj.prop.getFullname();
console.log(test); // John Doe
```

And the result is

```js
"Aurelio De Rosa"
"John Doe"
```

`"Aurelio De Rosa"` makes sense. But why `"John Doe"`? The reason is, `var test` is being set in the global scope so when executing the function, the context of `this` is set to a global scope. Versus the nested scope of simply getting the console log of `obj.prop.getFullname()`

Making a couple updates, we can use the `call()` method to change the scope of `test`, even though it is set in the global context. First we need to update `var test` to use the function itself as the value, not the return of the function. Then we can log out the result of that function using the `call()` method.

```
var test = obj.prop.getFullname;
console.log(test.call(obj.prop)); // Aurelio De Rosa
```

Let's try another variation on this example. By putting all of the `fullname` values inside the scope of of the object, we can easily log out the value of `fullname` from the function inside `getFullname`.

```js
var obj = {
   fullname: 'Colin Ihrig',
   getFullname: function() {
     return this.fullname;
   },
   prop: {
      fullname: 'Aurelio De Rosa',
      getFullname: function() {
         return this.fullname;
      }
   }
};

console.log(obj.prop.getFullname()); // "Aurelio De Rosa"
console.log(obj.getFullname()); // "Colin Ihrig"
```

As you would expect, since everything is being set within specific context. Even if you set a global var, like illustrated in the following code, since there are no global setters, you get the outcome you expect.

```js
var foo = obj.prop.getFullname();
console.log(foo); // "Aurelio De Rosa"
```

Using the `call()` method again, let's start with this example code where we have a global var of `fullname` and then the same `obj` var as before.

```js
var fullname = 'John Doe';

var obj = {
   fullname: 'Colin Ihrig',
   getFullname: function() {
     return this.fullname;
   },
   prop: {
      fullname: 'Aurelio De Rosa',
      getFullname: function() {
         return this.fullname;
      }
   }
};
```

Now, just like before, if I create a new variable in the global scope, the context of `this` will be in the outer global scope, right? Right.

```js
var foo = obj.prop.getFullname;
console.log(foo()); // "John Doe"
```

But when I use the `call()` or `apply()` method, I can force scope into the inner object. So right now, the value of `foo` is simply set to `obj.prop.getFullname`. If I use the `call()` method on the `foo` variable, I then force the context to be whatever is in the method call. For example, I want the scope of `this` to be within `obj.prop`.

```js
var bar = foo.call(obj.prop);
console.log(bar) // "Aurelio De Rosa"
```
