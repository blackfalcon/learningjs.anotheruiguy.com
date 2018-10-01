# What does 'strict' do?

Most recently you may have seen the following in the head of your JavaScript files

```js
`use strict`;
```

This is a feature introduced in ES5 that simply means, the JavaScript interrupter to run in STRICT mode. This doesn't change the output of your code nor does it allow for different features to work. It simply changes how the interrupter will run your code.

Let's look at a simple example.

```js
myNewVar = 'foo'
```

Now, there is nothing really wrong with this code, but it's bad practice. Setting the value for a global variable without first defining it's scope is the problem. Without setting `'use strict';`, the interrupter will __not__ throw an error.

To update this, we simply add `'use strict';` to the code.

```js
'use strict';

myNewVar = 'foo'
```

Now when you run the code, you will see an error.

```js
myNewVar is not defined
```

### Common uses

Remember, the `'use strict';` has to be the first line of code and it has to be a string as presented here. Most of the time you will see this as the first line of a large block of JavaScript. Keeping in mind, comments don't matter. An example is ...

```js
/*
  comments are good because they help
  people know that there is stuff to learn
*/

'use strict';

var globalvar = 'foo';
```

Another common use is to declare strict usage within the scope of a function itself. This may work best if you are building out a very modular architecture, for example ...

```js
/*
  comments are good because they help
  people know that there is stuff to learn
*/

function foo() {
  'use strict';

  var globalvar = 'foo';
}

foo();
```

This really is best because depending on how the modular code is compiled, you may or may not want to use strict mode globally. For example, the following code, while poor form, will not throw any errors.

```js
/* comments are good because they help
   people know that there is stuff to learn
*/

function foo() {
  'use strict';

  var globalVar = 'foo';
  console.log(globalVar);
}

newGlobalVar = 'foo';

foo();

console.log(newGlobalVar);
```

But stating strict mode in the global scope, the code will still run, but you will get the following error,

```js
ReferenceError: newGlobalVar is not defined
```

Keep in mind, this has a lot to do with the [hoisting](/thingsToKnow/hoisting.html) of variables.

### Silent failures

Another benefit of this feature is to help eliminate a common JavaScript frustration. That of `silent errors`. Those errors include trying to write properties to non-writable objects, trying to delete un-deletable properties, having duplicate keys in an Object Literal or using Octal Literals (numbers preceded by a `0`, like `020`).

For example, the following code will simply fail silently and the duplicate key's value will simply be over-written by the last key's value.

```js
var object = {
  car: true,
  make: 'Toyota',
  model: 'Sienna',
  car: false
}
```

Running this in strict mode should of course will throw an error of a duplicate key.

Using Octal Literals are used in JavaScript, but not supported in strict mode, for example,

```
var aNumber = 120 + 020;
```

Another silent killer can be using something like the `delete` operator incorrectly. In the following example you could try to [delete](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete) a whole object, but you can't do that. Nor can you delete a number, a string, or an array.

Without using strict mode, you would never know you wrote bad code, but you would know (maybe) that your object was never deleted.

```js
var foo = {
  make: 'ford',
  model: 'mustang'
}

delete foo;
```

 But adding `'use strict';` ...

```js
Uncaught SyntaxError: Delete of an unqualified identifier in strict mode.
```
