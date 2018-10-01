> {{book.title}} v{{book.version}}

# Destructuring

The destructuring assignment syntax is a completely new concept to JavaScript via the ES6 specification. Just looking at it, I feel that it makes little sense. But once you understand what it is trying to do, it all falls into place.

> The destructuring assignment syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

First, let's talk about the syntax. To the left you start with your declaration, be it `var`, `const` or `let`. Then you have your object or array literal syntax. Last, you point to the object or array you are destructuring.

What needs to be understood first is that the elements that will go into the object or array literal syntax is what will be unpacked from the original object or array and assigned to individual variables.

```js
declaration { ... } = object

or

declaration [ ... ] = array
```

Why would you use this? The main reason for the destructuring syntax is, once again, for the purpose of having less procedural code in favor of more declarative code. Like I said, once you understand the syntax you will come to look at the destructured expression and quickly understand what it is doing.
