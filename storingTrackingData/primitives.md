> {{book.title}} v{{book.version}}

# JavaScript Primitives

You may have heard the term `primitives` thrown around, and in an interview, it may be asked what they are. The simple truth is that there are two types of data in JavaScript; primitives, and objects.

Objects are things that contain simple or complex data. Objects have a special functions called methods. Methods can be used to discover information about the object itself.

Primitives, on the other hand, have no properties and their existence is sometimes the result of applying methods to an object. For example, look at the following list.

* undefined
* null
* boolean
* string
* number

These are not objects, but simply the literal return when performing an action.

### There are caveats

All laws may be broken and some rules may be bent. While it is true that primitives are not objects, you may be asking yourself ...

> If a string is a primitive, how come I can use `'abc'.length`?

Well, there is a wild card in JavaScript called `coercion`. JavaScript can, and will, coerce between primitives and objects. But what does that mean? Simply put, coercion, or type coercion in JavaScript, is a process of converting values into common data types to allow for certain operations. Coercion deserves its own deep-dive, [but someone else already wrote that](http://idiallo.com/javascript/type-coercion-conversion-in-javascript). Just know that JavaScript will do its absolute best to return a positive response.

Just understand that ...

```js
console.log('3' == 3); // true

console.log('3' === 3); // false

console.log(Number("0x11")); // 17 ††
```

... and you will sleep well at night.

†† it's a [math](https://stackoverflow.com/questions/6405033/c-0x11-in-decimal) thing
