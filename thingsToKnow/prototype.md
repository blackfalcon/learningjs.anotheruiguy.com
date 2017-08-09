# Prototype Object

The following example creates a prototype, Tree, and an object of that type, theTree. The example then displays the constructor property for the object theTree.

```js
function Tree(name) {
  this.name = name;
}

var theTree = new Tree('Redwood');
console.log('theTree.constructor is ' + theTree.constructor);
```

Explain this!
