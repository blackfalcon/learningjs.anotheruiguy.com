https://jsbin.com/duvoputapi/edit?js,console

Put something in the book that references timeout functions. There isn't a lot to unpack here, but it's something that has always kind of screwed with my head. 

```js
// This function is in the global space and 
// will execute without need to call the function 
setTimeout(function() {
  console.log('WAKE UP!')
}, 5000);


// This function is also in the global space and can be
// refrenced from anywhere in the app, but does require 
// a reference call 
function fooTime(time, str) {
  setTimeout(function() {
    console.log(str)
  }, time);
}

fooTime(1000, 'HOLY WOW');
```
