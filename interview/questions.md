# General Topic JavaScript questions

In preparing for the JavaScript interview, you need to be prepared to be hit from all angles of the JavaScript world, and it's a big ass world! Some questions are pretty straight forward and want to test your general knowledge of the language. Others are really out of left field and require a lot of specific domain knowledge and deep deep understanding of JavaScript from a Computer Science perspective.

Where you fall in that range of questions, I can't tell you. But what I can help with is documenting the questions I have heard and provide what I think is the best answer.

1. __What is the difference between `var` and `let`__

  1. The main difference between `var` and `let` is that `let` has `block scope`. What this means is that when you are using the `let` keyword, it will die inside the block that is defining it. This concept is called `garbage collection`. `var` on the other hand only respects `functional scope`.
  1. Another difference is that the `let` keyword does NOT get hoisted

1. __What is the difference between `let` and `const`__

  1. After the assignment of a `const` value, you CANNOT reassign a new value to that `const` name
  1. It's best practice to define Objects with `const` as you can update the contents of the object, but not reassign it's whole value

1. __ What is the difference between the `==` and the `===` [comparison](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Comparison_operators) operators?__

  1. `==` only compares values
  1. `===` compares value AND type

1. __What is the difference between `null` and `undefined`?__

  1. They both represent an empty value
  1. `undefined` is a state where you have defined a placeholder, but not yet defined it's value
  1. `null` is a falsy value
  1. `typeof(undefined) // undefined`
  1. `typeof(null) // object`

1. __What does the use of the arrow function `() =>` do?__

  1. With bubbling, the event is first captured and handled by the innermost element and then propagated to outer elements.
  1. With capturing, the event is first captured by the outermost element and propagated to the inner elements.
  1. Think  - 'trickle down, bubble up'
