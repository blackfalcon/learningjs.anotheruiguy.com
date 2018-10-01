> {{book.title}} v{{book.version}}

# Destructuring objects

Much in the same way that pulling values out of an array is a repetitive task, the same can be said for pulling values from an object. And again, destructuring can help. The following example illustrates the basic concepts around destructuring an object. What is important to understand is, unlike arrays, object destructuring is based on the object's keys.

## Beginner example

In the following example, I have a simple object that describes a new television.

```js
const obj = {
  manufacture: 'Samsung',
  size: '50-inch',
  resolution: '4K Ultra HD Smart LED',
  price: 527.99
}
```

Let's imagine that we have a function that consumes this object and creates a series of variables from it. These variable declarations may look like this.

```js
const manufacture = obj.manufacture
const size = obj.size
const resolution = obj.resolution
const price = obj.price
```

That's not too bad, but given the power of destructuring, we can actually do the same thing in one line. And remember, we need to use the same key name as it appears in the object in order to make the connection. But this also allows us to destructure in any order, unlike how arrays work.

```js
const {manufacture, price, size, resolution} = obj;
```

We can even destructure an array contained within an object. Let's update our object to have an array of outlets where this television can be bought.

```js
const obj = {
  manufacture: 'Samsung',
  size: '50-inch',
  resolution: '4K Ultra HD Smart LED',
  price: 527.99,
  retailers = ['Amazon', 'BestBuy', 'Costco']
}
```

Even though this array is within an object, we would still use the `[]` syntax to destructure the data and reference the specific object and key.

```js
const [one, two, three] = obj.retailers;
```

Now let's imagine that we have the URLs for each of the product pages on the retailer's sites.

```js
const obj = {
  manufacture: 'Samsung',
  size: '50-inch',
  resolution: '4K Ultra HD Smart LED',
  price: 527.99,
  retailers: ['Amazon', 'BestBuy', 'Costco'],
  purchaseOnline: {
    one: 'http://www.amazon.com',
    two: 'http://www.bestbuy.com',
    three: 'http://www.costco.com'
  }
}
```

Then to quickly and easily get these values, we could use the following code. Note that since these are object keys we are using the `{}` syntax.

```js
const {one, two, three} = obj.purchaseOnline;
```

## Slightly harder example

Taking what we learned above, let's create a few destructuring examples form a slightly more complex data object.

```js
const batman = {
  firstAppearance: 'Detective Comics #27',
  secretIdentity: 'Bruce Wayne',
  parents: ['Martha Wayne', 'Thomas Wayne'],
  villainsDetails: {
    theJoker: {
      firstAppearance: 'Batman #1',
      secretIdentity: 'unknown',
      powers: 'fearless criminal'
    },
    bane: {
      firstAppearance: 'Batman: Vengeance of Bane #1',
      secretIdentity: 'unknown',
      powers: 'intelligence and super strength'
    },
    catwoman: {
      firstAppearance: 'Batman #1',
      secretIdentity: 'Selina Kyle',
      powers: 'themed weapons, vehicles, and equipment'
    },
    madhatter: {
      firstAppearance: 'Batman #48',
      secretIdentity: 'Jervis Tetch',
      powers: 'brilliant \'neurotechnician\' with knowledge to dominate and control the human mind'
    },
    rasalGhul: {
      firstAppearance: 'Batman #232',
      secretIdentity: null,
      powers: 'advanced knowledge of hand-to-hand combat, chemistry, detective artistry, physics, and martial arts'
    }
  },
  powers: 'advanced technology'
}
```

Now, there are so many things we can do with this object, but for starters, let's get all the basic information about Batman.

```js
const firstAppearance = batman.firstAppearance;
const category = batman.category;
const secretIdentity = batman.secretIdentity;
const powers = batman.powers;
const mother = batman.parents[0];
const father = batman.parents[1];
```

Again, nothing too complex, but with destructuring, we can reduce this to two lines of code and get the same results.

```js
const [mother, father] = batman.parents;
// "Martha Wayne" "Thomas Wayne"

const {secretIdentity, powers, firstAppearance} = batman
// "Bruce Wayne" "advanced technology" "Detective Comics #27"
```

What also makes this syntax easy to work with is the ease that you can update the pointer inside the object itself for different data. In this example I went from `batman` to `batman.villainsDetails.catwoman` using the same variable assignments.

```js
const {secretIdentity, powers, firstAppearance} = batman.villainsDetails.catwoman;
// "Selina Kyle" "themed weapons, vehicles, and equipment" "Batman #1"
```

In addition to getting specific data from the object and easily binding those to variables, we can also iterate over the sub-objects within the parent object. For example, let's get the names of the keys for all the villains in the object.

```js
const [ one, two, three, four, five ] = Object.keys(batman.villainsDetails);
// "theJoker" "bane" "catwoman" "madhatter" "rasalGhul"
```

In this next example, we can use this syntax to quickly and easily bind the individual sub-objects within the Batman object to individual variables.

```js
const {madhatter, theJoker} = batman.villainsDetails;
```

And then even destruct from there.

```js
const {secretIdentity, powers, firstAppearance} = theJoker
// "unknown" "fearless criminal" "Batman #1"

const {secretIdentity, powers, firstAppearance} = madhatter
// "Jervis Tetch" "brilliant 'neurotechnician' with knowledge to dominate and control the human mind" "Batman #48"
```

## Destructuring combination syntax

Aside from individually parsing an object's data, you can easily combine statements in simpler statements or even a single statement. For example, I want to iterate through the `parents` array in the `batman` object. Then I want to assign the object's data for `theJoker` and `catwoman` to individual variables. Last, within `villainsDetails` I want to get the properties of the `madhatter` object.

Just for fun, let's long-hand all these variable assignments.

```js
const mother = batman.parents[0];
const father = batman.parents[1];
const theJoker = batman.villainsDetails.theJoker;
const catwoman = batman.villainsDetails.catwoman;
const firstAppearance = batman.villainsDetails.madhatter.firstAppearance
const secretIdentity = batman.villainsDetails.madhatter.secretIdentity
const powers = batman.villainsDetails.madhatter.powers
```

Now, let's do the same work, except using the destructuring syntax.

In this example, I am using the basic versions of the syntax as described above, notice the reference of nested properties of the object to the right.

```js
const [mother, father] = batman.parents
const {theJoker, catwoman} = batman.villainsDetails
const {firstAppearance, secretIdentity, powers} = batman.villainsDetails.madhatter
```

If you only want to reference the parent object, you can also reference the nested properties in the variable syntax to the let of the assignment operator.

```js
const {parents: [mother, father]} = batman
const {villainsDetails: {theJoker, catwoman}} = batman
const {villainsDetails: {madhatter: {firstAppearance, secretIdentity, powers}}} = batman
```

Knowing this, we also know that we can actually combine those three lines into a single statement.

```js
const {parents: [mother, father], villainsDetails: {theJoker, catwoman, madhatter: {firstAppearance, secretIdentity, powers}}} = batman
```

7 lines to 3, to 1. Destructuring is a super power.
