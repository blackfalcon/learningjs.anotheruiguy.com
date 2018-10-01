> {{book.title}} v{{book.version}}

# Objects

So, what makes an Object an object anyway? Well, there are some simple rules to follow, Objects have:

1. Properties (characteristics or keys)
1. Events (the point in which someone/something interacts with the code)
1. Methods (actions performed on the object's data)

The opposite of objects is [primitives](/primitives.html).

Two things that really jump out in regards to Objects are; objects have properties and methods that can be used to understand their properties. So, think of an Object as a 'thing' and the properties are the what describe the 'thing'.

## Object examples

For starters, the following examples could be considered objects as there are methods that could be performed on each to return a result.

```js
const foo = 'This is a string object';
const bar = ['this', 'is', 'an', 'array', 4, 'you'];

console.log(foo.length) // 23
console.log(bar.length) // 6
```

Then there is what is called an `Object Literal`. And that is anything that is inside `{ }`, for example;

```js
const foo = {
  propertyOne: 'value';
  propertyTwo: 'value';
}
```

## Ways to build an object

All of the preceding examples illustrate how an object can be created within the scope of creating a variable, but then there is creating an object using the `Object()` constructor as the following illustrates.

The new object of `myOtherCar` is instantiated by using the `Object()` constructor with the `new` keyword. To add properties to the new object, the dot syntax is used to define a new property and its value.

```js
const myOtherCar = new Object();
myOtherCar.make = 'Toyota';
myOtherCar.model = 'Sienna';
myOtherCar.year = 2008;
myOtherCar.color = 'black';
myOtherCar.type = 'mini van';
myOtherCar.issue = 'Needs new tires';
```

The same can also be accomplished by declaring an empty object, `const myOtherCar = {}`.

## Working with complex objects

To build out a data structure of objects inside an array, here is an example of how a mechanic's garage may be modeled out. The garage is the context of the array and each vehicle is an object in the mechanicGarage array.

```js
const mechanicGarage = [
  {
    make: 'Toyota',
    model: 'Sianna',
    year: 2008,
    type: 'van',
    color: 'Black',
    wheels: 4,
    doors: 5,
    issue: 'Problem with loud breaks'
  },
  {
    make: 'Ford',
    model: 'Mustang',
    year: 1977,
    type: 'car',
    color: 'Silver',
    wheels: 4,
    doors: 2,
    issue: 'Losses power in 5th gear.'
  },
  {
    make: 'Lamborghini',
    model: 'Veneno',
    color: 'Silver',
    type: 'car',
    year: 2017,
    wheels: 4,
    doors: 2,
    issue: 'It cost US$4.5mil ... what was I thinking?'
  },
  {
    make: 'Harley Davidson',
    model: 'V-Rod',
    color: 'Silver',
    type: 'motorcycle',
    year: 2010,
    wheels: 2,
    doors: 0,
    issue: 'Aftermarket rake modification'
  }
]
```
Now that the concept of a garage is created and it is full of vehicles, all kinds of options are available. For one, a simple function can be written that will use a method on the object array and return a value.

```js
console.log(mechanicGarage.length); // 4
```

Another option available is to write a loop that will return an inventory. In the following example the `Object.keys()` method is used on the `mechanicGarage` object, then a `.forEach()` loop is performed on each key in that object. In the loop, I am also creating an enumerator using the `objKey`.

Inside the loop, a series of variables are being created that binds the value of an enumerator's key value. Using that loop's instance, a new string is created before moving onto the next object on the array.

**Note:** `forEach()` is discussed in a later module.

```js
Object.keys(mechanicGarage).forEach(function (objKey) {
  const make = mechanicGarage[objKey]['make'];
  const model = mechanicGarage[objKey]['model'];
  const color = mechanicGarage[objKey]['color'];
  const year = mechanicGarage[objKey]['year'];
  const issue = mechanicGarage[objKey]['issue'];

  const makeString = `${year} ${color} ${make} ${model}: ${issue}`;
  console.log(makeString);
})
```

that returns ...

```
"2008 Black Toyota Sianna: Problem with loud breaks"
"1977 Silver Ford Mustang: Losses power in 5th gear."
"2017 Silver Lamborghini Veneno: It cost US$4.5mil ... what was I thinking?"
"2010 Silver Harley Davidson V-Rod: After market rake modification"
```

This is just a simple example of what can be done with a series of objects in an array.

## Objects are mutable

Objects are not read-only, you can add, edit, delete content from objects as well. For example, here is a simple single object that describes a single vehicle.

```js
var myCar = {
  make: 'Ford',
  model: 'Mustang',
  year: 1977,
  type: 'car',
  color: 'Silver',
  wheels: 4,
  doors: 2,
  issue: 'Losses power in 5th gear.'
}
```

Let's imagine that the specification for a function is to insert the `VIN ID` of a vehicle. Using the dot syntax on the object itself, in the following example, the code targets the `myCar` object and again using the dot syntax the new parameter of `vinId` is created and a value appended.

```js
myCar.vinId = '1HGBH41JXMN109186';
```

Viewing this object in the console, you would see:

```js
[object Object] {
  color: "Silver",
  doors: 2,
  issue: "Losses power in 5th gear.",
  make: "Ford",
  model: "Mustang",
  type: "car",
  vinId: "1HGBH41JXMN109186",
  wheels: 4,
  year: 1977
}
```

In the same vein, individual `key:value` items can be deleted from an object. For example, let's say that there is a specification to remove the `wheels` value from the object. This is done using the `delete` operator on the object and key.

```js
delete myCar.wheels;
```

## Multi-word parameters

There may be times when data is not as clean as the examples being used. In the following example there are some multi-word parameters in the object. To use them, they need to be wrapped in quotes.

```js
var myHouse = {
  'back door color': 'white',
  'front door color': 'black',
  windows: 10,
}
```

To access the `windows` key, simply write the following.

```js
console.log(myHouse.windows)
```

But to get the value of `back door color`, the dot syntax cannot be used as this is a string with spaces. To fix this, use the square-bracket`[]` syntax as illustrated in the next example.

```js
console.log(myHouse['back door color'])
```

## Object != Object

This is an interesting side-note to conclude this section. Imagine that there are two objects that have the exact same properties and the only difference is the name of the object. Are these objects equal to each other? In the following example, there are two objects being created, `obj` and `foo`. For each of these objects, their properties are exactly the same.

```js
const obj = new Object();
  obj.make = 'Toyota';
  obj.model = 'Scion';

const foo = {
  make: 'Toyota',
  model: 'Scion'
}
```

If these objects are logged to the console, the following would be returned.

```js
[object Object] {
  make: "Toyota",
  model: "Scion"
}
[object Object] {
  make: "Toyota",
  model: "Scion"
}
```

Assuming that if a comparison against these two objects were written, it would come back as `true`

```js
console.log(obj === foo) // false
```

A JavaScript object is ONLY equal to itself.

```js
console.log(obj === obj) // true
console.log(foo === foo) // true
console.log(obj === foo) // false
```

When trying to compare the contents of an object an easy way to do this is to use the `JSON.stringify()` function on each object


take the properties of the object and convert them into a JSON string array.

```js
objString = JSON.stringify(obj);
fooString = JSON.stringify(foo);

console.log(objString) // "{\"make\":\"Toyota\",\"model\":\"Scion\"}"
console.log(fooString) // "{\"make\":\"Toyota\",\"model\":\"Scion\"}"
```

With this done, the two new variables can be compared.
```js
console.log(objString === fooString) // true
```
