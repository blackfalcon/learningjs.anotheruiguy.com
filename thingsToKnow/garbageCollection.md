# Garbage Collection

The management of memory resources in JavaScript is pretty much automatic. With each line of code we write, we are creating primitives, objects, functions, etc ... and all of that takes up memory.

But what happens when the values of these 'things' are no longer needed? And how are things things determined to be no longer needed?

The primary concept we are dealing with here is `reachability`. How is reachability determined?

1. Are the variables global?
1. Are the variables, and parameters, local to the current function?
1. Are the variables, and parameters, of other functions in the current chain of nested calls?

Another criteria that determines a variable's reachability is, if there is an objects in a local variable, and that object has a property that is referencing an outside object, then by definition, that object is reachable.

Yeah, this is weird, so let's look at some examples.

Let's say that you create a simple object, as illustrated in the following example.

```js
let animal = {
  species: 'dog';
};
```

Then you come along and you change the property of the object ...

```js
animal.species = 'cat';
```

At this time, the property value of `dog` has lost its reference and is tossed into the garbage.

Adding a little complexity to this, let's say that we have still have the `animal` object and we want make that equal to a `pet` object. But then we come along and remove the reference of the `species` key, thus making it impossible to have that as a `dog` as a `pet`. I know, sad right?

```js
let animal = {
  species: 'dog'
};

let pet = animal;

delete animal.species;
```

By doing this, we have tossed the property of `species` into the garbage and because the two objects were equal to each other, they would both return as empty objects.
