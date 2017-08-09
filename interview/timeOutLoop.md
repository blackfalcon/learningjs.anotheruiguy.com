# Timeout Loop

```
function createTimeoutLoop() {
  let i = 0;

  for (i = 0; i < 3; i++) {
    setTimeout(function(x) {
      return function() {
        console.log(x);
      };
    }(i), 1000 * i);
  }
}

createTimeoutLoop();
```
