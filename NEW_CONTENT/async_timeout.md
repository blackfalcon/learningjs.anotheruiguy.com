https://www.pluralsight.com/guides/front-end-javascript/introduction-to-asynchronous-javascript

There is a lot to unpack from this article. One is the async examples below, then the next part is the understanding of Callback Hell and Promises.

Need to create a NEW section in the book for Async JavaScript with a sub-section that uses the content below called `timeout response` and then another sub-section for `Callbacks and promises`.



https://jsbin.com/gihigo/edit?js,console

/*
  Example of async JavaScript.
  The following code, while written in linier sequence, the log
  to the console will not happen in this order
*/

// Say "Hello." > Instant
console.log("Hello.");

// Say "Goodbye" > Appears five seconds from now
setTimeout(function() {
  console.log("Goodbye!");
}, 5000);

// Say "Hello again!" > Appears right after first "Hello."
console.log("Hello again!");


/*
  Running a timeout inside a for loop presents its challenges.
  What this code WILL do versus what you THINK it will do are different
*/

for(var i = 0; i < 4; i++) {
  setTimeout(function() {
    console.log(i + " second(s) elapsed");
  }, i * 1000);
}

/*
  The issus has to do with scope. While the timeout function prints to the log
  every second, the issue is that the value of i has ready been set to 4.

  To correct this, we need to save the value of i with each iteration of the loop.
  A classic way to address this is to maintain the value of i within a Closure.

  To do this, add in a self-executing function that takes in the argument of i.
*/

for(var i = 0; i <= 4; i++) {
  (function (arg) {
    setTimeout(function() {
      console.log(arg + " second(s) elapsed");
    }, arg * 1000); }
  )(i);
}

/*
  Nesting a Closure funciton will work, but that syntax can be VERY confusin.
  In ES6, instead of using VAR, using LET actually has block scoping.

  This means that by simply referring to i as LET versus VAR, the value if i
  will be maintained inside the setTImeout function block. There is no need
  for a Closure here.
*/

for(let i = 0; i <= 4; i++) {
  setTimeout(function() {
    console.log(i + " second(s) elapsed");
  }, i * 1000);
}


/*
  In this example, I was asked to fix the bug in this code so that
  console log doesn't wait 500ms before printing to the log.

  What's interesting is that it will iterate through all values, but it just
  holds onto that data for 500ms before printing to the console.

  I was asked to keep the setTimeout and for loop intact, but that seemed like a
  trick question O_O
*/

(function createTimeoutLoopQuiz() {
  setTimeout(function() {
    for (i = 0; i < 3 ; i++) {
      console.log('The broken value is: ' + i);
    }
  }, 500);
})();

/*
  One thing that seemed odd to me was that the for loop was inside the
  setTimeout function. Changing that put be back at a place where I was
  having an issue with scope.

  Wrapping the setTimeout again with a Closure helped address that.

  I also updated the timeout iterator.
*/

(function createTimeoutLoopClosure() {
  for(var i = 0; i < 3; i++) {
    (function (i) {
      setTimeout(function() {
        console.log('The Closure value is: ' + i);
      }, i * 1000)
    })(i);
  }
})();

/*
  And, last but not least, no Closure and using ES6 LET keyword to
  maintain the value of i within the block.
*/

(function createTimeoutLoopEs6() {
  for(let i = 0; i < 3; i++) {
    setTimeout(function() {
      console.log('The ES6 value is: ' + i);
    }, i * 1000)
  }
})();
