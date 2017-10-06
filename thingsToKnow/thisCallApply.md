# Invoked via .call() or .apply() methods

It should be noted that while the syntax of these two functions are almost identical, the functional difference between the two is, the `call()` method accepts an argument list, whereas the `apply()` method accepts a single array of arguments. In my mind, these differences are so small that I would default to using the `call()` method until there is a good reason to be more specific.

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
