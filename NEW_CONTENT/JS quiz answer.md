```
//
// NEW QUESTION
//

// Fix the following code so that the counter increments
// appropriately while keeping the setTimeout and for 
// loop intact.

// move the setTimeout function outside the loop,
// not suggested to run a function inside a loop
function createTimeoutLoop() {
  let i = 0;
  
  for (i = 0; i < 3 ; i++) {
    setTimeout(function() {    
      console.log('The value is: ' + i);
    }, 500)
  };
}

// the answer I think they were looking for 
function createTimeoutLoop() {
  
  // establish the for loop
  for (let i = 0; i < 3; i++) {
    
    // In the setTimeout function and pass in i
    setTimeout(function(i) { 
      
      // create a closure using i as the iterator
      return function() { 
        console.log('The value is: ' + i); 
      };
    
    // return i and then the time increment 
    // each loop return requires the value of 1000 * i
    // or all the values will be returned at the same time
    }(i), 1000 * i);
  }
}

createTimeoutLoop();


// call the function, which should print 0, 1, 2 to the console.
// createTimeoutLoop();
createTimeoutLoopFixed();

//
// NEW QUESTION
//

// Create a Person object with 2 methods, "setName" and "printName"
// that can be chained so that the code below can be run successfully.

// define person constructor 
function Person(name) {
  // use setName method to set name value to 
  // current this
  this.setName(name);
}

// extend person with printName method
Person.prototype.printName = function(name) {
  // console log current this.name;
  console.log(this.name);
  return this;
}

// extend person with setName method
Person.prototype.setName = function(name) {
  this.name = name;
  return this;
}

// Uncomment this code when you are ready:

// var person could be an empty object as long
// as you use the setName method
var person = new Person("Jill");
person.printName().setName("Tom").printName();
// This should output "Jill" and then "Tom" in the console.

// with ES6 this could also be written as a class

class PersonEs6 {
  constructor(name) {
    this.setName(name);
  }
  
  printName(name) {
    console.log(this.name);
    return this;
  }
  
  setName(name) {
    this.name = name;
    return this
  }
}

const personEs6 = new PersonEs6('');
personEs6.setName('Jill').printName().setName('Tom').printName();

//
// NEW QUESTION
//

// Array with Odd Numbers
// Write a program that returns an array that contains all the odd numbers
// of the array argument.

// Set array
const myArray = ([1,2,3,4,5,6,7]);

// build function to test if number in array
// is odd or not, set boolean value
function getOddNumbers(num) {
  return num % 2 !== 0; 
}


// set var to capture value of filtered numbers
let filteredArray = myArray.filter(getOddNumbers);

// console log var value
console.log(filteredArray);


// Example:
// getOddNumbers([1,2,3,4,5,6,7]); should return [1,3,5,7];
```