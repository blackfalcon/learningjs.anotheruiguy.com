> {{book.title}} v{{book.version}}

# Imperative(Procedural) programming

> A programming paradigm that uses statements that change a program's state. In much the same way that the imperative mood in natural languages expresses commands, an imperative program consists of commands for the computer to perform. Imperative programming focuses on describing how a program operates.

If you are new to JavaScript, or even a seasoned vet, the discussion around imperative programming may confuse you. Procedural programming is a type of Imperative programming and the terms are often synonymous, here I will illustrate what it really means to code in an Imperative/Procedural paradigm.

Imperative means to show your work. And what this can also mean is a lot of repeated work as your code is procedurally going through data and producing results. This code often becomes too tied to the function you are creating and an attempts to abstract for reuse is usually too difficult to do. In addition, having to write all this code, there is more opportunity for mistakes and unpredictable outcomes.

The last complaint you will hear about Imperative code is that it's hard to read. When reading Imperative code, as I am sure you have done already, means to have to read through the code and understand decipher what it is doing. This, of course, is in direct opposition to Declarative code.

In our example, I will need a handful of objects to work with.

```js
const vehicles = [
  {
    name: 'V-Wing Fighter',
    military: 'Galactic Empire',
    use: 'Shuttle security'
  }, {
    name: 'N-1 Starfighter',
    military: 'Nabooian',
    use: 'Combat Starfighter'
  }, {
    name: 'Republic Attack Cruiser',
    military: 'Galactic Republic',
    use: 'Military transport'
  }
];
```

Next, we need a function where we can pass in a vehicle name, test if that name is in the array of objects and then print out the data. The Imperative approach is to use a `for()` loop to parse out the objects in the array, then when a match is found, push data from the object to a new array and return.

```js
function printVehicles(vehicle) {
  let newArray = [];

  for (let i = 0; i < vehicles.length; i++) {
    if (vehicles[i].name === vehicle) {
      newArray.push(vehicles[i].name, vehicles[i].military, vehicles[i].use);
      break;
    } else {
      newArray = [vehicle, 'No data']
    }
  }
  return newArray;
}

```

I'd argue that this example is still good functional programming, but using the Imperative paradigm we introduce opportunity for error and inability for easy reuse.

```js
console.log(printVehicles('V-Wing Fighter')); // ["V-Wing Fighter", "Galactic Empire", "Shuttle security"]
console.log(printVehicles('Foo Fighter')); // ["Foo Fighter", "No data"]
```

But hey ... it works! So you are still winning.
