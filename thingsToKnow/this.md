# this

`this` is one of the more confusing aspects of JAvaScript to a new comer. For the simple fact, that this is a very abstract concept.

To understand `this`, just remember; 'Context is King!'

Simply put, `this` is a contextual reference pertaining to the action being performed. The `this` keyword can be used to access values, methods and other objects that are context specific.

There are 4 ways that a `this` will get it's value

1. Within the context of a function
1. Within Methods and Objects
1. Within a Constructor
1. When a function is invoked via `.call()`, `.apply()` and `.bind()`

### Context within a function

The simplest way you understand how `this` works is by using the context of an event. Let's say for example you have a series of DOM elements that with a `click` event you want to change the text. But, there are multiple DOM elements and you don't want to put a unique ID on each one.

In the following example we make use of some basic JavaScript features to find all the DOM elements, loop through them, and use the context of `this` to apply the action to only the element that is being interacted with.

To start out, let's build our DOM.

```html
<body>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
  <p class="foo">This is foo bro</p>
</body>
```

Pretty straight forward. Now to get an array of all the DOM elements that we want to be able to click on.

```js
// find all the buttons with this class
var elements = document.getElementsByClassName('foo');
```

Next we can simply loop through the array and apply a click event listener. The magic here is that we are using the keyword of `this` to basically say, "The element that you click on, change `this` text."


```js
// loop through array and add the event of 'click' that will toggle a CSS class
for (var i = 0; i < elements.length; i++) {
  elements[i].addEventListener('click', function() {
    this.innerHTML = "Paragraph changed!";
  });
}
```

To see this in action, check out [this JSBin](https://jsbin.com/voziguc/edit?html,js,output).



### Within Methods and Objects

...



### Within a Constructor

...



### Invoked via .call() or .apply() methods

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

### Invoked via a .bind() method
