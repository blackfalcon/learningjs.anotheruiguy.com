# Build an Object with Methods

There is a lot to unpack in this one. Especially with the advent of ES6, the answer can actually go in two different directions based on your knowledge and understanding of JavaScript. So, here is the question.

> Create a Person object with 2 methods, "setName" and "printName" that can be chained so that the code below can be run successfully. The expected outcome is that in the console we should see; "Tom" "Jerry"

Just to clarify this. In the solution to this question, we are to create the `setName` and `printName` methods. These are not default JavaScript methods. This may be confusing as there is a `setName()` method on Java. Just more noise to confuse you.

The following code is the code that we are required to run in order for our function to work.

```js
// Create the object person using the Person constructor
var person = new Person("Tom");

// Having created the new person object, apply the printName() method
// Using the setName() method, change the value of person object and again, print out using the printName() method
person.printName().setName('Jerry').printName();
```

As you can see from the example given, you can keep chaining methods one after another to change the value of the person object.

What this questions is really going after is; what is your understanding of [Objects](/thingsToKnow/objects.html), [Constructors](/thingsToKnow/funcMethodConst.html) and JavaScript's prototypical architecture. I remember being asked this question, as a take home question thank [insert preferred deity], but none the less I was taken back and had to do some real research and phone a friend too.

And once you get into this, there is a bonus round. How can this be made better using the new ES6 Class architecture? BOOM!

### The answer ...

Or at least, this is the answer I came up with, but first some minor review. Once you create a prototype object, you can call methods on the prototype to perform actions. If you don't remember, please go back and review [Constructors](/thingsToKnow/constructors.html). That being said, the answer to this question is a little hard to specify the answers in a linear statement, so we will be doing a little round-robin with this one.

To start off, we do need to create our `Person` constructor so that we can create a new `person` object. The trick is that we will not define the guts of this constructor until we have created the other methods as we need to reference the methods in the creation of the object. What does that look like?

```js
function Person(name) {
  ...
}
```

Ok, now that we have the scaffolding of a constructor we know that when we create the object from that constructor we will have access to its prototype. So, [methods](/thingsToKnow/methods.html), as you may remember, in order to create a new method we need to construct a function that targets its prototype.

To do this, I am going to create the method of `setName()` that will add the value of `name` to the `Person` prototype. As for the guts of this method, we need this to take the argument of `name` and set that to the `name` value of the object. Then to get this value out of the function, we need to return the value of `this`.

But why not just the value of `name`? That is because we want the properties object, not just the value.

```js
Person.prototype.setName = function(name) {
  this.name = name;

  return this;
}
```

Last, we need to create the `printName` method much in the same way that we created the `setName()` method. For this block of code, we are going to give it the instruction to log the name to the console. After that action, we need to return the `this` object back out of the method.

```js
Person.prototype.printName = function(name) {
  console.log(this.name);

  return this;
}
```

Seeing how we have created the methods that we can use on the `person` object to set the name and then print out the name, we can go back to the `Person` constructor and fill that in.

In a typical constructor we need to use a construct like `this.key = value` to create the key:value in the object. In this example, the value will be taken from the `name` argument, and the object's value is going to be set using the `setName()` method inside the object constructor.

```js
function Person(name) {
  this.setName = name;
}
```

Great! Now we can run the code that creates the name and prints it out to the console.

```js
var person = new Person("Tom");
person.printName().setName('Jerry').printName();
```

Keep in mind, that we don't need to set a value for new Person right away. We could set that as an empty string and then use the `setName()` method right away.

```js
var person = new Person("");
person.setName('Tom').printName().setName('Jerry').printName();
```

It should also be noted that the life cycle of this object means that the `name` value is being set with one command, and then replaced with the next. There is no persistence of this value other than the last `setName` use.

## ES6 for the win!

Once I got my head wrapped around how this works, I started remembering back to what I had learned about the new concept of `Classes` in ES6 and the `constructor()` method. JavaScript has never had the concept of classes in JavaScript until now. But the example above was the hack that developers have used to create the existence of classes.

Just a quick overview. A `class` in JavaScript will consist of a few things. It's name, a constructor, and its method(s). In the previous execution of this code we created the `Person` constructor. This translates to the name of the Class in this context.

```js
class Person {
  ...
}
```

Pretty straight forward. Next, inside this class block we need use the `constructor()` method to create our object the same way we did before. But before we get there, let's build out the methods we need. First, let's create the `setName()` method. Instead of creating a function in a global space, attaching it to the prototype like we did before, we can not crate this function inside the scope of the class.

```js
class Person {
  setName(name) {
    this.name = name;
    return this;
  }
}
```

Same goes for our `printName()` method as well.

```js
class Person {
  setName(name) {
    this.name = name;
    return this;
  }

  printName(name) {
    console.log(this.name);
    return this;
  }
}
```

All looking pretty familiar? So, for our last trick of the night, we will build out the constructor for this class the same way we did in the previous example, leveraging the methods we created in this class.

```js
class Person {
  constructor(name) {
    this.setName(name);
  }

  setName(name) {
    this.name = name;
    return this;
  }

  printName() {
    console.log(this.name);
    return this;
  }
}
```

And there you have it, an ES6 class. And we can run the same code as we did before.

```js
const person = new Person("Tom");
person.printName().setName("Jerry").printName();
```

But wait! You may be looking at this and saying, "_But we are changing the value of the name key inside the object, why are we using const?_"

Yes, when using `const` you are not allowed to change the type or value of a variable in your script, but you can change the propertied of an object. So it's actually considered best practice to create all your objects using the `const` keyword so that you are alerted if the type of this variable's value changes in your script.
