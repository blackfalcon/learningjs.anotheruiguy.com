# Template Literals

What are template literals? Well, while they are another annoyingly names JavaSscript thing, they are actually really cool and a welcome addition to the JavaScript language. Template literals is a great way to generate strings in JavaScript interwoven with data. When creating a string, instead of using quotes `""` or `''`, you first wrap your sting with the backtick <code>`</code> then any data pulled into the template you wrap within the expression syntax, `${}`.

Before we get into how to use template literals, let's look at the old skool way of creating strings. We've all seen this before, even if you don't know much about JavaScript you have seen something like this. In the following example we have a simple object that has properties related to our family pet. Then I have a simple function that I can pass that object into and it will parse the data and output a string.

```js
const familyPet = {
  name: 'Harley',
  species: 'dog',
  color: 'black and white',
  breed: 'Shih Tzu'
}

function foo(obj) {
  let str = 'Our pet ' + obj.species + ' is a ' + obj.color + ' ' + obj.breed + ' named ' + obj.name;
  return str;
}

console.log(foo(familyPet));
```
If you are like me you have HATED This syntax. Things like `+ ' ' +` makes my skin crawl. I mean, we can put men on the moon but we can't crate space in a string without having to put a physical empty space string in there?

Well, good news. We can put men on the moon and it turns out we a learning how to use computers too. Let's take the previous example and replace that old string concatenation process and use template literals.

```js
function foo(obj) {
  let str = `Our pet ${obj.species} is a ${obj.color} ${obj.breed} named ${obj.name}.`
  return str;
}
```

Ahhh ... that just feels so much better. This is but scratching the surface of what you can do with template literals, but even this simply application is am amazing improvement from what we had before.
