> {{book.title}} v{{book.version}}

# Avoiding conditionals

Conditional statements in JavaScript are very simple to use and very powerful in determining the outcome of your code. But beware, too many `if()` statements or logic branching in your code can also greatly complicate your application and introduce opportunities for bugs. In those cases, instead of using branching logic, you could look at using other strategies.

## Stocking shelves

Let's imagine that we need to build an app that assists people in restocking shelves at a grocery store. The app would respond to scanning in the SKU of a product and return the info we need for stocking.

For starters, use a constructor for generating all the product data.

```js
// our Grocery constructor
function Grocery(name, category, perishable = false) {
  this.name = name;
  this.category = category;
  this.perishable = perishable;
}
```

Next, let's create a few products.

```js
const SKU110011 = new Grocery('Blue Bunny', 'frozen foods');
const SKU99876 = new Grocery('Harvest Wheat Bread', 'bakery', true);
const SKU33574 = new Grocery('Farm fresh eggs', 'dairy', true);
```

Now that we have some product data in our app, we need a function where we can pass in the SKU and return the product's name, area of the grocery store to stock and special instructions if it is perishable or not. The following example of the `stockShelf()` function is pretty simple and seems like a logical solution, but there are potential issues.

```js
function stockShelf(item) {
  let isle = 0;
  let isPerishable = '';

  if (item.category === 'frozen foods') {
    aisle = 12;
  } else if (item.category === 'bakery') {
    aisle = 9;
  } else if (item.category === 'dairy') {
    aisle = 2;
  }

  if (item.perishable) {
    isPerishable = ' ' + `Perishable, stock according to Exp. date`
  }

  let statement = `${item.name}: ${item.category}, aisle ${aisle}.`;

  console.log(statement + isPerishable)
}

stockShelf(SKU110011)
stockShelf(SKU99876)
stockShelf(SKU33574)
```

Looking at this code it seems reasonable to have a series of `if()` statements to determine what aisle a product goes in based on its product type. But how extensible is this logic? How many aisles are in your grocery store? End caps, top shelving, bins, etc ... It's easy to see that this code does not scale.

### The refactor

Instead of writing a complex series of conditionals, you may consider constructing a series of functions that return values based on the input. Also, what about of putting aisle data into the conditional? That data is best served by a structure that can be read versus questioned.

To begin our refactoring, let's create an object to host all the aisle data.

```js
// Store stocking object
const grocerAisles = {
  'frozen foods': '12',
  'bakery': '9',
  'dairy': '2'
}
```

For the `stockShelf()` function we simply need to accept the object as an argument like the previous function. For starters, this function begins to create the string we need to be printed back to the UI.

```js
// Get stocking info function
function stockShelf(item) {
  const statement = `${item.name} goes in ${item.category}.`
}
```

Here we can remove the `if()` logic from the previous version and replace with a single function. In the previous function, we took the value of `item.category` and ran it through all the `if()` statements. Using the `grocerAisles` object we can construct a function that passes in the value of `item.category` and returns the aisle value.

```js
// getAisle function used as callback to get grocery aisle from product type
function getAisle(category) {
  return grocerAisles[category]
}
```

To make use of this, within the `stockShelf()` function we can use a callback in the `statement` variable `${getAisle(item.category)}`.

```js
function stockShelf(item) {
  const statement = `${item.name} goes in ${item.category}, aisle ${getAisle(item.category)}.`
}
```

To determine if a product is perishable or not, we are going to use similar logic as we did before, but put this into a function that asks a simplified question using a conditional (ternary) operator. This expression simplifies the verbose `if()` statements and reduces the opportunity for extensive logic branching and errors.

To keep from having to pass around object variables, this function is using the `this` keyword.

```js
// Test if product is perishable
function perishable() {
  return this.perishable ? ' ' + `Perishable, stock according to Exp. date` : '';
}
```

With this `perishable()` function we can now finish the `stockShelf()` function. On our `console.log()` statement we can call the `perishable()` function and using the `.call()` method we can pass in `item` and get the additional string if true.

```js
// Get stocking info function
function stockShelf(item) {
  const statement = `${item.name} goes in ${item.category}, aisle ${getAisle(item.category)}.`
  console.log(`${statement}${perishable.call(item)}`);
}
```

The final version of the code is in the following example. What makes this more desirable is the independent functions with reduced logic/complications.

```js
// our Grocery constructor
function Grocery(name, category, perishable = false) {
  this.name = name;
  this.category = category;
  this.perishable = perishable;
}

// Store stocking object, product type : aisle
const grocerAisles = {
  'frozen foods': '12',
  'bakery': '9',
  'dairy': '2'
}

// Get grocery aisle from product type
function getAisle(category) {
  return grocerAisles[category]
}

// Test if product is perishable
function perishable() {
  return this.perishable ? ' ' + `Perishable, stock according to Exp. date` : '';
}

// Produce string(s) based on product object
function stockShelf(item) {
  const statement = `${item.name} goes in ${item.category}, aisle ${getAisle(item.category)}.`
  console.log(`${statement}${perishable.call(item)}`);
}

// Product definitions
const SKU110011 = new Grocery('Blue Bunny', 'frozen foods');
const SKU99876 = new Grocery('Harvest Wheet Bread', 'bakery', true);
const SKU33574 = new Grocery('Farm fresh eggs', 'dairy', true);

// Execution
stockShelf(SKU110011)
stockShelf(SKU99876)
stockShelf(SKU33574)
```
