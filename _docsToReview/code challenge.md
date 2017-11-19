// Fix the following code so that the counter increments
// appropriately while keeping the setTimeout and for 
// loop intact.

function createTimeoutLoop() {
  var i = 0;
  
  setTimeout(function() {
    for (i = 0; i < 3 ; i++) {
      console.log('The value is: ' + i);
    }
  }, 500);
}

// call the function, which should print 0, 1, 2 to the console.
// createTimeoutLoop(); 

//
// NEW QUESTION
//

// Create a Person object with 2 methods, "setName" and "printName"
// that can be chained so that the code below can be run successfully.

//////////////////////////////
// ENTER YOUR SOLUTION HERE //
//////////////////////////////

// Uncomment this code when you are ready:
// var person = new Person("Jill");
// person.printName().setName("Tom").printName();
// This should output "Jill" and then "Tom" in the console.

//
// NEW QUESTION
//

// Array with Odd Numbers
// Write a program that returns an array that contains all the odd numbers
// of the array argument.

var getOddNumbers = function (num) {
  return num % 2 !== 0;
};

var myArray = [1,2,3,4,5,6,7];
console.log(myArray.filter(getOddNumbers));

// Example:
// getOddNumbers([1,2,3,4,5,6,7]); should return [1,3,5,7];