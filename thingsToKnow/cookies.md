# Cookies

Ahhh cookies, the tried and true, the old standard for storing and retrieving data on your user's device/browser. Nothing new here, in fact, cookies have been around pretty much since the beginning of the browser. While there is a long history with Cookies and their implementations, here I am going to focus on how to create, set and retrieve based on some simple actions using JavaScript.

## Setting and Getting cookies

Using cookies is pretty simple, there are two basic functions that we need to use and those are `setCookie()` and `getCookie()`. I am sure from the names you can easily understand their roles.

### Set the cookie

For starters, let's create the `setCookie()` function. This function will accept two arguments, one for `cookieName` and then one for `cookieValue`.

```js
function setCookie(cookieName, cookieValue) {
  ...
}
```

Next, add the code for setting the expiration date. This is somewhat tedious in JavaScript, but the steps are as follows. Create a new variable for `today` and set that to a new `Date()` prototype.

Now that we have `today`, we need to get a value for `today` plus 6 months. To do this, we set another variable using the new `Date()` prototype, but inside that, using the `today` value, get the current time (which will be returned in milliseconds) and do some simple math that will add 6 months worth of milliseconds to today's date.

If you `console.log()` these two variables, you will simply get an object back. And, milliseconds are not human-readable, so you could append the `.toGMTString()` for each variable and that will return a value that you can read.

```js
function setCookie(cookieName, cookieValue) {
  const today = new Date();
  const expiry = new Date(today.getTime() + 1000 * 60 * 60 * 24 * 180);
}
```

Great! We have done the hard part, now to actually set the cookie to the user's browser. Due to this we will use the `document.cookie` method and set a text string with the info we have.

`cookieName` will be provided at the time the function is being used. For the `cookieValue` we will use the `escape()` function to remove any `%20` from a string that may be passed as the cookie value. Setting the path to the root of the domain being used, and then setting our expiration date.

```js
function setCookie(cookieName, cookieValue) {
  const today = new Date();
  const expiry = new Date(today.getTime() + 1000 * 60 * 60 * 24 * 180);

  document.cookie = `${cookieName}=${escape(cookieValue)}; path=/; expires=${expiry.toGMTString()}`
}
```

And that's it. You now have a reusable function that can be used to set cookies when needed.

### Get the cookie

Setting a cookie is the easy part, but if you can't get the cookie, then this data is useless. Good news, getting the cookie is just as easy as setting it.

To start out, let's create a new function that uses one parameter, that if the cookie name we are looking for.

```js
function getCookie(cookieName) {
  ...
}
```

To get things going, we need three variables. `name`, `decodedCookie` and `cookieArray`.

To really understand what is happening here, you need to understand how cookies are stored in the browser. For example, say we have three cookies storied per our domain. They are ...

* `'beatles', 'love love me do'`
* `'king of rock', 'Elvis'`
* `'king of pop', 'Michael Jackson'`

If you were to `console.log(document.cookie);` then you would see a return of ...

```
beatles=love%20love%20me%20do; king of rock=Elvis; king of pop=Michael%20Jackson
```

Knowing this, we can now talk about our three variables. Setting `name` to the `cookieName` entered as an argument of the function and adding the `=` operator will set a value to be something like `beatles=` as previously illustrated.

For the `decodedCookie` variable, this uses the `decodeURIComponent` to strip out the `%20` characters from the returned cookie string and return ...

```
beatles=love love me do; king of rock=Elvis; king of pop=Michael Jackson
```

Last, we want to create an array of the cookies returned, so we use the `split()` method to do this. If we were to console this out, we would see ...

```
["beatles=love love me do", " king of rock=Elvis", " king of pop=Michael Jackson"]
```

Summed up, our function would start out looking like the following code.

```js
function getCookie(cookieName) {
  const name = `${cookieName}=`;
  const decodedCookie = decodeURIComponent(document.cookie);
  const cookieArray = decodedCookie.split(';');
}
```

At this point, we have retrieved the cookie data from the client and now can use this information to see if the cookie we are looking for is in the returned string. To do this, we will use a `for()` loop with some nested loops to iterate over the newly created `cookieArray`.

Looking at the following code there is a `while (cookie.charAt(0) == ' ')` statement. What is that all about? Look back a step at the array we created. notice things like `" king of rock=Elvis"`? We would probably never have a cookie named `" king of rock"`, so this empty space is left over from the cookie string we got back from the browser. This `while` loop is designed to detect that space and if found, move onto the next character and find the name of the cookie we are looking for. Myself, I never understood that block of code until I manually logged out the array and saw the space.

The last part of this function is to return an empty string `return ''` just in case the cookie you requests is not there. If you don't add this, you will run this code in an infinite loop and create a singularity.


```js
function getCookie(cookieName) {
  const name = `${cookieName}=`;
  const decodedCookie = decodeURIComponent(document.cookie);
  const cookieArray = decodedCookie.split(';');

  for (let aCookie in cookieArray) {
    let cookie = cookieArray[aCookie];

    while (cookie.charAt(0) == ' ') {
      cookie = cookie.substring(1);
    }
    if (cookie.indexOf(name) == 0) {
      return cookie.substring(name.length, cookie.length);
    }
  }
  return '';
}
```

## How to use?

Great, we have very useful and reusable JavaScript functions for setting and getting cookies from our user's client. But, how do we make use of these?

To do this, let's imagine a scenario where we want to set a cookie called `showUtilityNav` and it's value is `true` post a user's election to show a utility bar in their site. Then, when the user returns to this site, we want to check for this coolie and hide or show the utility bar, depending on the existence of the cookie.

You could nest this inside any other function, but the idea is to test a condition and then if true, use the `setCookie()` function and pass in the cookie name and its value.

```js
if (utilityNav) {
  // Set cookie name and its value in the client
  setCookie('showUtilityNav', true);
}
```

On the flip side, when a user returns to that view, we want to test for that cookie and perform the correct action based on that preference.

In this example, let's imagine that the utility nav bar is hidden by default, and the experience is that the users elect to see it. When the user comes to the view, we will test for this preference setting and if found, run a function that will expand the utility bar.

To keep this encapsulated, I illustrate this action as an [IIFE](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression).

```js
(function checkCookie(myCookie) {
  const utilityBarPref = getCookie(myCookie);

  if (utilityBarPref) {
    expandUtilityBar();
  }
})('showUtilityNav');
```

## In conclusion

To wrap this up, you can see that there is little mystery to setting or getting cookies in any setting. But before you go about and use cookies for every little thing in your app, be aware that there are major security flaws in this legacy technology. Please do not use this for setting any preference around logins or any kind of access to confidential information. These things are best-set server side and out of the reach of the user's ability to view and hack on the code in the client.
