# So Truthy or Falsy you say?

This again is one of those weird JavaScript things. Because coercion is a thing, nothing is really full `true` or `false`. Simply put, unless you are being explicit about your values, values are liable to be coerced. So, what are the 6 things that are Falsy?

1. `false`
1. `0`
1. `""`
1. `null`
1. `undefined`
1. `NaN`

On the flip side, what constitutes Truthy?

1. `" " ` // even a space is considered a string
1. `{}`
1. `[]`
1. `function() {}`

There is a matrix of comparisons that can be drawn between these types and not to mention the array of opportunity when you use either `==` or `===`. Like the following.

```js
console.log(false == []); // true
console.log(false === []); // false
```

And remember, `NaN` is equal to nothing, not even another `NaN` value. Take the following example where we create two variable objects using the same bad math operators and primitive values. They both equal `NaN`, but when we compare those two together, it comes back as false.

```js
var foo = '12' * 18 * 'foo'; // NaN
var bar = '12' * 18 * 'foo'; // NaN
console.log(foo === bar); // false
```

But wait! There is something that little know about. You can convert values to real Booleans using the `!!` assignment operator. This basically takes any truthy or false return and makes it either `true` or `false`. So in the following example you can see how we simply log `!!foo === !!bar` and we get a `true` return.

```js
var foo = '12' * 18 * 'foo'; // NaN (false)
var bar = '12' * 18 * 'foo'; // NaN (false)
console.log(!!foo === !!bar); // true
```

Same as ...

```js
var foo = 1 + 1; // 2  (true)
var bar = 2 + 2; // 4  (true)
console.log(!!foo === !!bar); // true
```

And ... the nightmares return.
