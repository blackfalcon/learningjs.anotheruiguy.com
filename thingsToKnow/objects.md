# Objects

In order for you to even get your head wrapped around objects. Objects is where it's at and it's a that and a bag of chips. So, what makes an Object an object anyway? Well, there are some simple rules to follow, Objects have:

1. Properties (characteristics or keys)
1. Events (the point in which someone interacts with the computer)
1. Methods

The opposite of objects are [primitives](/thingsToKnow/primitives.html).

The two things that really jump out at me in regards to Objects are that they have properties and you can use methods on them to understand their properties. So, think of an Object as a 'thing' and the properties are the what describe the 'thing'. And you will mostly encounter Objects in the form of an Array of Objects that help describe a larger context.

## Object examples

For starters, anything that is appended to a variable is an object. Like the following examples;

```js
var foo = 'This is a string object';
var bar = 120;
var baz = ['this', 'is', 'an', 'array', 4, 'you'];
```

Then there is what is called an `Object Literal`. And that is anything that is inside `{ }`. For example;

```js
var foo {
  'thing1': 'value';
  'thing2': 'value';
};
```

## Ways to build an object

Creating an object is pretty simple. All of the preceding examples illustrate how you can create an object within the scope of creating a variable. Pretty straight forward. But then there is creating an object using the `Object()` constructor as illustrated in the following example.

```js
var myOtherCar = new Object();
myOtherCar.make = 'Toyota';
myOtherCar.model = 'Sienna';
myOtherCar.year = 2008;
myOtherCar.color = 'black';
myOtherCar.type = 'mini van';
myOtherCar.issue = 'Needs new tires';
```

You could also accomplish the preceding code by simple declaring an empty object, `var myOtherCar = {}` and that would work too.

## Working with complex objects

Knowing this, we can make things a little more exciting and build out a data structure of objects inside an array. For example, how would you model out a mechanic's garage? The garage is the context of the array and each vehicle is an object in the model, I mean garage.

```js
var mechanicGarage = [
  {
    'make': 'Toyota',
    'model': 'Sianna',
    'year': 2008,
    'type': 'van',
    'color': 'Black',
    'wheels': 4,
    'doors': 5,
    'issue': 'Problem with loud breaks'
  },
  {
    'make': 'Ford',
    'model': 'Mustang',
    'year': 1977,
    'type': 'car',
    'color': 'Silver',
    'wheels': 4,
    'doors': 2,
    'issue': 'Losses power in 5th gear.'
  },
  {
    'make': 'Lamborghini',
    'model': 'Veneno',
    'color': 'Silver',
    'type': 'car',
    'year': 2017,
    'wheels': 4,
    'doors': 2,
    'issue': 'It cost US$4.5mil ... what was I thinking?'
  },
  {
    'make': 'Harley Davidson',
    'model': 'V-Rod',
    'color': 'Silver',
    'type': 'motorcycle',
    'year': 2010,
    'wheels': 2,
    'doors': 0,
    'issue': 'After market rake modification'
  }
];
```

Ok, now that we have this concept of a garage full of vehicles, we can do all kinds of things. For one we can write a simple function that will use a method on the object array and get back a count.

```js
console.log(mechanicGarage.length);
```

We can also write a loop that will return an inventory. Let's unpack this first. In the following example I am using the `Object.keys()` method on the `mechanicGarage` object, then performing a `.forEach()` loop on each key in that object. In the loop, I am also creating an enumerator using the `objKey`.

Next, inside the loop I am creating a series of variables that binds the value of an enumerator's key value. Then using that loop's instance to build out a string and then move onto the next object on the array.

```js
Object.keys(mechanicGarage).forEach(function (objKey) {
  var make = mechanicGarage[objKey]['make'];
  var model = mechanicGarage[objKey]['model'];
  var color = mechanicGarage[objKey]['color'];
  var year = mechanicGarage[objKey]['year'];
  var issue = mechanicGarage[objKey]['issue'];


  var makeString = `${year} ${color} ${make} ${model}: ${issue}`;
  console.log(makeString);
});

```

that returns ...

```
"2008 Black Toyota Sianna: Problem with loud breaks"
"1977 Silver Ford Mustang: Losses power in 5th gear."
"2017 Silver Lamborghini Veneno: It cost US$4.5mil ... what was I thinking?"
"2010 Silver Harley Davidson V-Rod: After market rake modification"
```

This is just a simple example of what we can do with a series of objects in an array, of course there are thousands of other examples we could go through.

## Objects are mutable

Objects are not read only, you can add, edit, delete content from objects as well. For example, let's go back to a simple single object array that describes a single vehicle.

```js
var myCar = {
  'make': 'Ford',
  'model': 'Mustang',
  'year': 1977,
  'type': 'car',
  'color': 'Silver',
  'wheels': 4,
  'doors': 2,
  'issue': 'Losses power in 5th gear.'
}
```

Now let's say that we need to insert the VIN ID of the vehicle. We can use this using the `.` syntax on the object itself. In the following example, I am targeting the `myCar` object and using the `.` syntax I can create the new key type of `vinId` and also append the value.

```js
myCar.vinId = '1HGBH41JXMN109186';
```

When you run this code, you would see the following output in the console.

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

In the same vein, you can delete individual key:value items from the object as well. For example, let's say that we need to remove `wheels` value from the object. To do this we use the`delete` operator on the object and key.

```js
delete myCar.wheels;
```

## Multi-word keys

There may be times when you get data that is not as clean as the examples we have been using. Like the following example, we see that there are some multi-work keys on our object.

```js
var myHouse = {
  'back door color': 'white',
  'front door color': 'black',
  'windows': 10,
}
```

To access the `windows` key, we could simply write the following.

```js
console.log(myHouse.windows)
```

But to get the value of `back door color`, we can't use the `.` syntax as this is a string with spaces. To fix this we use the `[]` syntax, as illustrated in the next example.

```js
console.log(myHouse['back door color'])
```

## Object != Object

This is an interesting side-note to conclude this section. Let's say that you have two objects that have the exact same properties. The only difference is the name of the object. Are these objects equal to each other? Let's take the following example.

```js
var obj = new Object();
  obj.make = 'Toyota';
  obj.model = 'Scion';

var foo = {
  'make': 'Toyota',
  'model': 'Scion'
}
```

If we log these objects in the console, we would get the following output as expected.

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

So, you would assume that if we write a comparison operator `===` against these two objects it would come back as `true`, right? WRONG!

```js
console.log(obj === foo) // false O_O
```

A JavaScript object is ONLY equal to itself. What we are trying to compare is the contents of the object and an easy way to do this is to take the properties of the object and convert them into a JSON string array.

```js
objString = JSON.stringify(obj);
fooString = JSON.stringify(foo);
```

Now that we have done this, we can write a comparison between the two and it will come back as `true` in this case.

```js
console.log(objString === fooString) // true
```
