> {{book.title}} v{{book.version}}

# The value of this

JavaScript's concept of `this` is one of the more confusing aspects of JavaScript to not only a new developer, but an experienced one as well.

Compared to other languages, `this` may behave differently. It also has different behavior characteristics when your code is running in strict mode versus non-strict mode. These points alone can be extremely confusing.

The value of `this` may be determined by how the function is called on your app. The value of `this` can't be assigned during execution, but may be different each time that functions is called. To add some confusion to this already mysterious concept, ES5 gave us the `bind()` method and allows developers to assign the function's value of `this` regardless of how it's called and ES6 introduced arrow functions that do not have their own `this` property and maintain the scope from which they came from.

To really understand `this`, just remember; 'Context is King!'


## Lesson outline

By the end of this module, students will have an understanding of the following topics

* `this` within the context of a function
* `this` within Methods and Objects
* `this` within a Constructor
* `this` when a function is invoked via `.call()`, `.apply()` and `.bind()` methods
* `this` within a Arrow function
