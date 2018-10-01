# Timeout Loop

```js
(function createTimeoutLoop() {
  for (let i = 0; i <= 60; i++) {
    setTimeout(function(tick) {
      return function() {
        console.log(`${tick} ${tick === 1 ? 'second' : 'seconds'}`);
      };
    }(i), 1000 * i);
  }
})();
```
