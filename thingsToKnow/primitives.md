# JavaScript Primitives

You may hear the term `primitives` thrown around, and in an interview you may be asked what they are. The simple truth is that there are two types of data in JavaScript, primitives and objects.

Objects, as we have come to know very well, are things that contain simple or complex data. Objects have a special power, and that is the ability to apply methods on the objects to discover its data's properties.

Primitives on the other hand have no properties and their existence is sometimes the result of applying methods to an object. For example, look at the following list.

* undefined
* null
* boolean
* string
* number

These are not objects, but simply the literal return when performing an action.

### There are caveat

All laws may be broken and some rules may be bent. While it is true that primitives are not objects, you may be asking yourself ...

> If a string is a primitive, how come I can use `'abc'.length`?

Well, there is a wild card in JavaScript called `coercion`. JavaScript and, and will, coerce between primitives and objects. But what does that mean? Simply put, coercion, or type coercion in JavaScript, is a process of converting values into common data types to allow for certain operations. Coercion deserves its own deep-dive, [but someone else already wrote that](http://idiallo.com/javascript/type-coercion-conversion-in-javascript). Just know that JavaScript will do its absolute best to return a positive response.

Just understand that ...

```js
console.log('3' == 3); // true

console.log('3' === 3); // false

console.log(Number("0x20")); // 32 O_O
```

... and you will sleep well at night.
