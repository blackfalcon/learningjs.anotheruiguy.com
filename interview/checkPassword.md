# Password validation

```js
function password(password) {
  const symbols = new RegExp('[^A-Za-z0-9_]');
  let success = false;

  for (let i = 0; i < password.length; i++) {
    let character = password.split('')[i];

    if (symbols.test(character) === true && password.length >= 10) {
      success = true;
    }
  }

  return success;
}

console.log(password('password0!'));
console.log(password('Dale'));
```

Getting crazy with regular expressions


```js
function newPassword(password) {

  var exp = /^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[^A-Za-z0-9_])(?=.{10,})/;
  return exp.test(password);

}

console.log(newPassword('Password0!'));
```
