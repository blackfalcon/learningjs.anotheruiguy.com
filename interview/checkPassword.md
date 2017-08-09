# Password validation

```js
function password(password) {
  var passwordLength = password.length;
  var passwordArray = password.split('');
  var symbols = new RegExp('[^A-Za-z0-9_]');
  var success;

  for (i = 0; passwordLength > i; i++) {
    var character = passwordArray[i];
    var symbolTest = symbols.test(character);

    if (symbolTest === true && passwordLength >= 10) {
      success = true;
      break;
    } else {
      success = false;
    }
  }

  return success;

}

console.log(password('password0!'));
```



```js
function newPassword(password) {

  // var exp = new RegExp("^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[^A-Za-z0-9_])(?=.{10,})");
  var exp = /^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[^A-Za-z0-9_])(?=.{10,})/;
  var test = exp.test(password);

  return test;

}

console.log(newPassword('Issaquah01'));
```
