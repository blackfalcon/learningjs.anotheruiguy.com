> {{book.title}} v{{book.version}}

# Invoked via .call() or .apply() methods

It should be noted that while the syntax of these two functions is almost identical, the functional difference between the two is, the `call()` method accepts an argument list, whereas the `apply()` method accepts a single array.

## Quick intro to .call() and .apply()

Before we get too far into examples, let's briefly explore what the `.call()` and `.apply()` methods really do. As previously stated, the `.call()` and `.apply()` methods pretty do the same thing, it's just that the `.call()` method operates on individual arguments and the `.apply()` method operates on argsArray. What does that mean? Here are the specs of both methods. What's consistent is the use of thisArg which will refer to the data in which you need to bind to.

```js
// the call() method
function.call(thisArg, arg1, arg2, ...)

// the apply() method
func.apply(thisArg, [argsArray])
```

#### The call() method

The `.call()` method is defined as the following:

> The `call()` method calls a function with a given `this` value and arguments provided individually.

What does that mean? To put it simply, the `.call()` method will call a function that consists of a reference to `this` but is void of context. Using this method we can set the scope of `this` to an object we want to use the values of, and then apply the arguments to the function.

I like Mad Libs and they make really great examples, so let's imagine that we have a program that will allow you to create a simple Mad Lib that has two parts to do this. One is to use a pre-defined object of words, and the other is to supply two arguments for additional words.

For the first part, here are our pre-defined objects.

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

For the second part we have a function that will have the `this` value. This will print out our Mad Lib. In this function, we see that there are references to `this` properties, but there is no context of `this` so these will return `undefined` if we try to run this function alone and simply add the two individual arguments.

```js
function printMadLib(nounOne, nounTwo) {
  return `Our school cafeteria has really ${this.adj} food. Just thinking about it makes my stomach ${this.verb}. The hamburgers taste like ${this.noun}. My friend Dave likes the ${nounOne} and thinks that it's made of ${nounTwo}.`
}

printMadLib('frog', 'rocks');
// "Our school cafeteria has really undefined food. Just thinking ..."
```

Using the `.call()` method we can use the same function, but reference the data from one of the pre-defined objects and get a better return. Notice how  `thisArg` is replaced with the reference to one of the pre-defined objects with the data we want.

```js
printMadLib.call(madLibsOne, 'frog', 'rocks');
// "Our school cafeteria has really slimy food. Just thinking about it makes my stomach wiggle. The hamburgers taste like Texas. My friend Dave likes the frog and thinks that it's made of rocks."

printMadLib.call(madLibsTwo, 'frog', 'rocks');
// "Our school cafeteria has really purple food. Just thinking about it makes my stomach rain. The hamburgers taste like dove. My friend Dave likes the frog and thinks that it's made of rocks."
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
"Our school cafeteria has really slimy food. Just thinking about it makes my stomach wiggle. The hamburgers taste like Texas. My friend Dave likes the farmer and thinks that it's made of car."
"Our school cafeteria has really purple food. Just thinking about it makes my stomach rain. The hamburgers taste like dove. My friend Dave likes the television and thinks that it's made of fireplace."
```

#### The spread syntax

The spread syntax has nothing to do with `this`, but it is worthwhile bringing up here as the functionality is very similar to the `.call()` method.

In ES6 this new syntax for passing arguments into a function was introduced. While the spread syntax has many uses, one that is interesting and best noted here is its ability to pass an array into a function without having to use the `.apply()` method.

For example, let's imagine that we have a function where you want to accept an array of items and two individual arguments. Madlibs!

```js
function printMadLib(args, adverb, pluralNoun) {
  return `My name is ${args[0]} and I am ${args[1]} yrs old. I own an ${args[2]} ${args[3]}. I ${adverb} eat ${pluralNoun}.`
}
```

Using what we learned from using the `call()` method is that we can create a function that will take an array as an argument. We could do that here as well, but the syntax is a little weird as we are not passing in an object to define the value of `this`. You could pass in `this` or `null` for the first parameter of the `call()` method, then the array and then the individual arguments.

```js
console.log(printMadLib.call(null, myArr, 'wholeheartedly', 'toads'));
```

In code, weirdness is a hack. Using the spread syntax is more fitting for this function, but we need to make a few updates. First, we need the spread operator of `...`. Then the spread argument needs to be the last parameter in your function.

```js
function printMadLib(adverb, pluralNoun, ...args) {
  return `My name is ${args[0]} and I am ${args[1]} yrs old. I own an ${args[2]} ${args[3]}. I ${adverb} eat ${pluralNoun}.`
}
```

When we call this function we need to pass in the arguments first and then the array with the spread syntax again. Without that the array will return as one item.

```js
console.log(printMadLib('wholeheartedly', 'toads', ...myArr));
// "My name is Tom and I am 12 yrs old. I own an orange car. I wholeheartedly eat toads."
```

## Further exploration of this

To further explore these methods, let's take a look at a slightly more complex example. Here I am creating two objects; the variable `fullname` and then the variable object of `obj`. Take a look at this code and then look at the outcome, the question is; how?

```js
var fullname = 'Bruce Wayne';

var obj = {
   fullname: 'Clark Kent',
   prop: {
      fullname: 'Diana Prince',
      getFullname: function() {
         return this.fullname;
      }
   }
};

let localScope = obj.prop.getFullname();
console.log(localScope); // "Diana Prince"

let globalScope = obj.prop.getFullname;
console.log(globalScope()); // "Bruce Wayne"
```

But why? The reason is, `globalScope` is being set in the global scope, so when executing the function, the context of `this` is set to a global scope. Versus the nested scope of simply getting the console log of `obj.prop.getFullname()`

The differences in the code are very subtle and easily missed if you are not watching. When setting the value of `localScope` I want the result of the function, this is why it is written with the `()` parens at the end of the statement.

Likewise, with setting the `globalScope` value, I am not including the `()` parens at the end of the statement, I am requesting that the function itself be returned. When I console.log `globalScope` I now include the `()` parens and execute the function in the global space. It's a bit to follow, I know.

Hold on, we haven't even gotten to the part about `call()` or `apply()` yet. Just setting the stage.

Remember, the value of `globalScope` is set to `obj.prop.getFullname` and when loosely bound, it will apply the value it sees fit to apply to `this`. Also keep in mind that if the global `var fullname = 'Bruce Wayne';` had not been set, this would return as `undefined`. Making a small update to this statement, I will use the `call()` method on the `globalScope` variable to specifically point to a property inside the object, `obj.prop`.

In essence, the `call()` method is pushing the scope from a globally applied value to the local scope of the object. FYI, the `apply()` method would work exactly the same in this example.

```
let globalScope = obj.prop.getFullname;
console.log(globalScope.call(obj.prop)); // "Diana Prince"
```

Let's try another variation on this example. By putting all of the `fullname` values inside the scope of the object, we can easily log out the value of `fullname` from the function inside `getFullname`.

```js
const obj = {
   fullname: 'Clark Kent',
   getFullname: function() {
     return this.fullname;
   },
   prop: {
      fullname: 'Diana Prince',
      getFullname: function() {
         return this.fullname;
      }
   }
};

const scopeOne = obj.prop.getFullname();
const scopeTwo = obj.getFullname();

console.log(scopeOne); // "Diana Prince"
console.log(scopeTwo); // "Clark Kent"
```

As you would expect since everything is being set within the specific context and you get predictable results. Even when setting a global var, as illustrated, since there are no global setters, you get the outcome you expect.

Using the `call()` method again, let's start with updating the example where we have a global var of `fullname` and then the same `obj` var as before.

```js
var fullname = 'Barry Allen';

const obj = {
   fullname: 'Clark Kent',
   getFullname: function() {
     return this.fullname;
   },
   prop: {
      fullname: 'Diana Prince',
      getFullname: function() {
         return this.fullname;
      }
   }
};
```

Now, just like before, if I create a new variable in the global scope, the context of `this` will be in the outer global scope, right? Right. Also, don't forget to pay attention to where the `()` parens are.

```js
const whatScope = obj.prop.getFullname;
console.log(whatScope()); // "Barry Allen"
```

Given this, when I use the `call()` or `apply()` method, I can force scope into the inner object. So right now, the value of `whatScope` is simply set to `obj.prop.getFullname`. If I use the `call()` method on the `whatScope` variable, I then force the context to be whatever is in the method call. For example, I want the scope of `this` to be within `obj.prop`.

```js
const thatScope = whatScope.apply(obj.prop);
console.log(thatScope) // "Diana Prince"
```

## Final notes

Here is all that code in one block for you to review.

```js
var fullname = 'Barry Allen';

const obj = {
   fullname: 'Clark Kent',
   getFullname: function() {
     return this.fullname;
   },
   prop: {
      fullname: 'Diana Prince',
      getFullname: function() {
         return this.fullname;
      }
   }
};

const scopeOne = obj.prop.getFullname();
const scopeTwo = obj.getFullname();

console.log(scopeOne); // "Diana Prince"
console.log(scopeTwo); // "Clark Kent"

const whatScope = obj.prop.getFullname;
console.log(whatScope()); // "Barry Allen"

const thatScope = whatScope.apply(obj.prop);
console.log(thatScope) // "Diana Prince"
```

I also want to point out the uses of `var` or `const` is not by accident. I will cover this later on, but the use of these variable keywords have a direct impact on scoped and alter the output of this code.
