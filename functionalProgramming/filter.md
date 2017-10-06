# Stop looping and start filtering

Latest developments in JavaScript has given rise to popular concepts in functional programming and something that is gaining a lot of attention as well is the `filter()` method. Really what this comes down to is that the traditional `for()` loop is great and extremely versatile, but at the same time can be extremely complex as in order for you to understand what the loop is doing, you gave to manually read through all the steps to summarize an answer.

Functional programming on the other hand goes in the opposite direction and declarative approach. That of a multitude of single responsibility functions and methods. One of those is the `filter()` method. As MDN puts it ...

> The `filter()` method creates a new array with all elements that pass the test implemented by the provided function.

So, what does that mean? Let's unpack this and get a better understanding of the `filter()` method.

## Old Skool Loops brah!

Let's start with an object called `vehicles`, and in this object we have a series of cars and trucks.

```js
const vehicles = [
  {
    'make': 'Toyota', 'model': 'Sienna'
  }, {
    'make': 'Toyota', 'model': 'Rav4'
  }, {
    'make': 'Honda', 'model': 'CR-V'
  }, {
    'make': 'Honda', 'model': 'Civic'
  }, {
    'make': 'Ford', 'model': 'F-150'
  }, {
    'make': 'Honda', 'model': 'Accord'
  }, {
    'make': 'Subaru', 'model': 'Forester'
  }, {
    'make': 'Ford', 'model': 'Mustang'
  }
]
```

Great! Now, let's create a function that will dig into this array of objects and return only the requested data. For starters we need to define the function that clearly states its intention.

```js
function printVehicles(vehicle) {
 ...
}
```

Next we need to establish the variable for the new array we will crate from this loop.

```js
function printVehicles(vehicle) {
  let newArray = [];
}
```

Now for the `for()` loop. All the standard stuff here as we would come to expect, so nothing special about looping through an object.

```js
function printVehicles(vehicle) {
  let newArray = [];

  for (let i = 0; i > vehicles.length; i++) {
    ...
  }
}
```

Inside this `for()` loop we need to ask a question. Is the vehicle in the object? If so, pass in its make into the new array. To do this, remember we are iterating through the `vehicles` object, so the question is, `if vehicles[i].make = vehicle`, right? `i` being the iterator and `.make` being the object key.

If that is true, then what we want in return is the model of the vehicle, so to do this we want to pass in `vehicles[i].model`.

In JavaScript the `.` syntax extends the scope of what we are looking at in the object. Say we had a key for `year` and by stating `vehicles[i].year` we would only get the years back of the vehicles.

```js
function printVehicles(vehicle) {
  let newArray = [];

  for (let i = 0; i > vehicles.length; i++) {
    if (vehicles[i].make === vehicle) {
        newArray.push(vehicles[i].model);
    }
  }
}
```

Given what we have here, we can run the following code and get a response.

```js
printVehicles('Ford'); // ["F-150", "Mustang"]
```

This all works really well and in this example you can probably quickly understand what this is doing. But what if it was even clearer?

## The Filter function

Using the same vehicles object, lets rewrite that `printVehicles()` function to be more declarative, this time using the `filter()` function. Much like the previous example, we will create the new function and inside this function I will declare the variable `auto`. Its value is the filtered function on the `vehicles` object where the `make` is that of `arg`.

```js
function printVehicles(arg) {
  let auto = vehicles.filter(function(vehicle) {
    return vehicle.make === arg;
  })

  console.log(auto)
}

printVehicles('Honda');
```

Running what we have here, this will return the filtered objects from the `vehicles` array of objects.

```js
[[object Object] {
  make: "Honda",
  model: "CR-V"
}, [object Object] {
  make: "Honda",
  model: "Civic"
}, [object Object] {
  make: "Honda",
  model: "Accord"
}]
```

That in itself is pretty awesome. With a very simply line of code we are easily filtering out the elements from our object that we were looking for. To take this up a level and get the same output as we had with the previous `for()` loop, we need to add a little more code and introduce another tool, the `map()` function.

## The Map function

Using the `filter()` function only gets you so far. In order to replicate what we had done in the previous `for()` loop example, we need to take this a step further. The `map()` function, as MDN puts it ...

> `map` calls a provided `callback` function once for each element in an array, in order, and constructs a new array from the results.

What this is telling us is that `map()` alone will take the result of the `filter()` and pass this into an array. Basically transforming the data.

`map()` by itself is pretty powerful. Take the following example where I make a simple function using the `map()` function to loop through the content in the object and return a template string.

```js
function foo(arg) {
  let str = '';

  arg.map(function(auto) {
    str += `The ${auto.model} is a ${auto.make}.\n`
  });

  return str;
}

console.log(foo(vehicles));
```

The output from this function would be something like;

```js
"The Sienna is a Toyota.
The Rav4 is a Toyota.
The CR-V is a Honda.
The Civic is a Honda.
The F-150 is a Ford.
The Accord is a Honda.
The Forester is a Subaru.
The Mustang is a Ford.
"
```

Like all other examples here, we can do powerful things without the assistance of any traditional `for()` loops.

Again, this is pretty cool, but this is not the result we were looking for. In the following example we can append the `map()` function to the result of the `filter()` function and return the value of `vehicle.model`.

```js
function printVehicles(arg) {
  let auto = vehicles.filter(function(vehicle) {
    return vehicle.make === arg;
  })
  .map(function(vehicle) {
    console.log(vehicle.model);
  })
}

printVehicles('Honda');
```

This code will print our each vehicle model to the console as separate lines.

```js
"CR-V"
"Civic"
"Accord"
```

This is like super powers using `filter()` and `map()` in the same function. We can easily filter out the parts of the array that we want and then output the content of that data into a new format. We are getting so close, but that's not what we want. How about we bring back the `newArray` concept like we used in the traditional `for()` loop example? Then inside `map()` function we can add the value of `vehicle.model` on each iteration.

```js
function printVehicles(arg) {
  let newArray = [];

  let auto = vehicles.filter(function(vehicle) {
    return vehicle.make === arg;
  })
  .map(function(vehicle) {
    newArray += `${vehicle.model} `;
  })

  console.log(newArray);
}

printVehicles('Honda'); // "CR-V Civic Accord "
```

Getting so much closer. This concatenates the results of the loop, but it's not in an array. We could do a `split(' ')` here, but ugh ... Knowing that we are getting the individual returns from the object, just like the `for()` loop example, I'd say it's best we use the `push()` function to get all the values into an array.

```js
function printVehicles(arg) {
  let newArray = [];

  let auto = vehicles.filter(function(vehicle) {
    return vehicle.make === arg;
  })
  .map(function(vehicle) {
    newArray.push(vehicle.model);
  })

  console.log(newArray);
}

printVehicles('Honda'); // ["CR-V", "Civic", "Accord"]
```

Now one could argue, that this is MORE code ... but this is more expressive code and one does not need to mentally parse through this function to discover what it is doing. Using these well defined functions one can simply read the code and understand its output.
