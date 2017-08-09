# Functional Programming in JavaScript

Here is the thing. You’ve probably already been doing functional programming all along. You ever use jQuery? Ever use Sass? Ruby or Python? Then you already understand the basic concepts behind functional programming, you just need to learn how to do this in JavaScript.

### Higher Order Functions

What really grinds my gears is when I am in a conversation with a JavaScript developer and I can't rattle off the names of higher order functions in JavaScript, or I can't give a good example of using `filter()` versus a for loop, and my contributions to this conversation are dismissed. Well, no more. And to be perfectly honest, those conversations are just chest beating contests really. "I know more cool words then you do ... " Whatever.

Just so that we have our heads wrapped around what is really happening here, the trick when it comes to higher order functions is the fact that in JavaScript a function can take another function as an argument. That is pretty huge when you think about it. JavaScript treats functions as 1st Class Citizens. What that means is that functions are treated as objects. That's how you can assign a function as the value of a variable and you can also pass them around just like any other reference. Think back to the examples in [Curring](/thingsToKnow/currying.html).

```js
var baz = foo(5);
var fooBar = baz(5)
console.log(fooBar); // 10
```

Really, another example of higher order functions is one you are probably really familiar with. Ever have a functional response to a click event? Of course you have. Take the following example,

```js
const eventTrigger = document.getElementById('click-me');

eventTrigger.addEventListener('click', function() {
  console.log(`You clicked on element ID ${this.id}`);
});
```

Pretty straight forward right? The function `.addEventListner()` accepts a function as an argument. In this instance we are creating a function expression on the fly and defining the outcome of this event all within context. But what if we wanted this console message on other elements in the view? Duplicating this would make your app A MAD HOUSE!!!!

Looking at the following example, let's imagine that there are two DOM elements where we want the same functionality. To do this we remove the use of the function expression and make it the declaration of `consoleClick()`. Now to bind this function to the click events, we reference that function as the second argument in the `addEventListener()` function. I am pretty sure that if you are learning this, you are only learning the name that goes with something that you have already been doing.

```js
const eventTrigger = document.getElementById('click-me');
const otherEventTrigger = document.getElementById('click-there');

function consoleClick() {
  console.log(`You clicked on element ID ${this.id}`);
}

eventTrigger.addEventListener('click', consoleClick);

otherEventTrigger.addEventListener('click', consoleClick);
```

I should point out that I am NOT passing in `consoleClick()`, but only `consoleClick`. This is not a typo, it's very much intentional. The reason is that when referencing functions you can either reference the function object or the result of the function. So my referencing `consoleClick` we are specifically saying that we want the whole function to be returned and it will run in this context. And for the smart ones out there ... notice the reference to `this` as well.

## Conclusion

So there you have it. In a nutshell, functional programming and the user of higher order functions is probably a best practice you have seen over time learning JavaScript. It's just that it has a better name now. The newness of functional programming is interesting to me, as I feel that if you have ever done any kind of programming at all, you are suspect to have used functional programming methods. It just makes sense IMHO. After years of being told to keep our code DRY, how else would you do that?

What makes higher order functions and functional programming such news is the advent of ES6 and its new super powers. ES6 is littered with new and powerful functions already baked into the language like `filter()`, `map()` and `reduce()`. But these are not new concepts. Things like `reverse()` have been around since the origin of the language.

Now that you have been given this knowledge, go out and be functional!
