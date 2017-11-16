# Date and Time

If you are like me, Date and Time using JavaScript has always been a confusing mess. There are a lot of really bad examples of how to use Date and Time in JavaScript, not to mention a lot of really bad examples out there describing how to get a dynamic date into the footer of your site for the copyright date.

So, before we get too far into this, let's make sure we understand what the JavaScript `Date()` prototype is. It is ... nothing. It only represents what will be captured. Remember, time keeps on ticking ticking ticking into the future ... So by calling `new Date()` you are creating a new captured instance in time. And from that instance, we can do all kinds of things. I won't go through and list out the whole API, as you can find that on MDN, but let's work out an example.

## Prettify the date

In this example, we will see how we get the current date and time and how we can break it apart and create a custom presentation of the date and time.

For starters, getting the current time from the browser is pretty simple, looking below you see how I created a new variable called `now` and its value is `new Date();`. That's it. By running that code we have a date. And, believe it or not, there is a super simple method we can run on the new date object and this will convert all the millions of milliseconds into a human-readable format. This is `toString()`.

```js
const now = new Date();
const nowStr = today.toString();

console.log(nowStr); // "Wed Nov 15 2017 14:29:45 GMT-0800 (PST)"
```

This will work in a pinch for sure, but not sure about you, but I want a little more control over my presentation. To do this, there is a long way and then there is the new ES6 short way.

#### The long way ...

Yes, this is the long and most procedural way, but it's guaranteed to work and gives you a lot of flexibility to do anything you want with it. For starters, let's parse out that date object and find the date, the day of the week, the month and the year.

```js
const now = new Date();
const nowStr = now.toString();
const nowDate = now.getDate();
const nowDay = now.getDay();
const nowYear = now.getFullYear();
const nowMonth = now.getMonth();

console.log(nowStr);
console.log(nowDate); // 15
console.log(nowDay); // 3
console.log(nowYear); // 2017
console.log(nowMonth); // 10
```

Boom, that's pretty easy! But ... for the day we get back `3`, and for a month we get back `10`. Day, that makes sense, it's the 3rd day of the week. Month, that's one off because the calendar starts at `0`, not `1` in JavaScript. That's ok, this will be something we can take advantage of next.

So, for these days, we want `Wednesday` and `November`. How do we do this? This is pretty easy really. I've seen some pretty insane ways to do this, but the easiest way I've done this is to create two arrays, one for Month and one for Day. This is static and I don't think that we need to worry about the days or months changing anytime soon.

The next thing we do is simply take that integer returned and use that as a pointer for the array. E.g. `3` === `Wednesday`. I updated the code below to use `days[now.getDay()]` and `months[now.getMonth()]`.

```js
const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday'];

const now = new Date();
const nowStr = now.toString();
const nowDate = now.getDate();
const nowDay = days[now.getDay()];
const nowYear = now.getFullYear();
const nowMonth = months[now.getMonth()];

console.log(nowStr);
console.log(nowDate); // 15
console.log(nowDay); // "Wednesday"
console.log(nowYear); // 2017
console.log(nowMonth); // "November"
```

This is pretty cool so far. In just a few steps we have grabbed the current date/time and converted this into something that we can easily use in building a new UI. In fact, we are pretty much there, so let's build out a string and log that to the console.

```js
const str = `${nowDay} the ${nowDate} of ${nowMonth}, ${nowYear}`

console.log(str); // "Wednesday the 15 of November, 2017"
```

This looks GREAT, except ... `Wednesday the 15`? How do we get the `th` in there? As far as I know, this is not a native function of JavaScript, but we can easily build this. Let's build a new function called `ordinalSuffix()` and it takes a single integer as a parameter. Reading through the code we see that it is running a few math functions on the value of `i`. First, we need to get the modulo of `10` and `100` per the value of `i`. Then depending on the modulo value, we return a new string that will have the `st`, `nd`, `rd` or `th` added to it. Pretty simple.

```js
function ordinalSuffix(i) {
  const modulo10 = i % 10;
  const modulo100 = i % 100;
  if (modulo10 == 1 && modulo100 != 11) {
      return `${i}st`;
  }
  if (modulo10 == 2 && modulo100 != 12) {
      return `${i}nd`;
  }
  if (modulo10 == 3 && modulo100 != 13) {
      return `${i}rd`;
  }
  return `${i}th`;
}
```

To use this, let's update the string we are building with `${ordinalSuffix(nowDate)}`.

```js
const str = `${nowDay} the ${ordinalSuffix(nowDate)} of ${nowMonth}, ${nowYear}`; // "Wednesday the 15th of November, 2017"
```

This is looking really awesome and I hope you are seeing how easy it is to manipulate this output into however you want this string to read in the UI.

The last thing I want to get in there is the current time. Now, time is a little tricky and to get this in here, we need to dip into ES6 land.

### The short way ...

Ok, we did the long way and from that, we got a really cool result. But to get the current time in there, I am stopping at the long way because converting milliseconds into human time is so last year. Let's talk about `toLocaleString()`. This is a pretty cool new Date/Time API and gives us some interesting superpowers.

First, super easy. Let's log this to the console and see what we get.

```js
console.log(now.toLocaleString()); // "11/15/2017, 3:00:53 PM"
```

Wow, that's kind of cool. And right away, this is something you might be able to use in a UI with no extra effort. Now, this method takes two parameters, region, and options. The region is pretty easy, for the most part, you would use `en-us`, and this is default. And for options, there is a really [huge list](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString) of options that you can read up on. For the sake of brevity, let's look at the following example.

```js
const standardOptions = {
  weekday: "long",
  year: "numeric",
  month: "short",
  day: "numeric",
  hour: "2-digit",
  minute: "2-digit",
  timeZoneName: 'short'
};
```

I am sure you can read this and this makes sense. To make use of this, our `console.log()` statement would be the following.

```js
console.log(now.toLocaleString('en-us', standardOptions)); // "Wednesday, Nov 15, 2017, 3:04 PM PST"
```

WOW! This is pretty amazing and almost makes the previous work seem utterly worthless. But it's not, I assure you in most UI design cases. But what we are really interested in here is that time! How do we get only that as a return? Here I introduce the `toLocaleTimeString()` method. Sure we can run that method without any arguments, but you will get back the seconds and we don't want that.

I will create a new options object for time and it would look like the following. Having that, I can create a new `console.log()` statement that uses the `toLocaleTimeString()` method and passes in the `timeOptions` object.

```js
const timeOptions = {
  hour: "2-digit",
  minute: "2-digit",
  timeZoneName: 'short'
};

console.log(now.toLocaleTimeString('en-us', timeOptions)); // "3:08 PM PST"
```

And there it is. An easily, human readable timestamp. Great. Now, let's backtrack to the previous code and add in a timestamp and see how that looks.

### Wrapping it all up.

At this point, we have discussed a lot of code. What I want to happen next is being able to add `It is ${now.toLocaleTimeString("en-us", timeOptions)}` to the output string. To make that happen, we need to pull in that `timeOptions` object and use the `toLocaleTimeString()` method on our `now` time object. Phew ... words!

```js
// Our months and days of the week
const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday'];

// Parsing the date object to populate all the date variables
const now = new Date();
const nowStr = now.toString();
const nowDate = now.getDate();
const nowDay = days[now.getDay()];
const nowYear = now.getFullYear();
const nowMonth = months[now.getMonth()];

// Utility function that will convert an integer into a string with the appropriate ordinal suffix
function ordinalSuffix(i) {
  const modulo10 = i % 10;
  const modulo100 = i % 100;
  if (modulo10 == 1 && modulo100 != 11) {
      return `${i}st`;
  }
  if (modulo10 == 2 && modulo100 != 12) {
      return `${i}nd`;
  }
  if (modulo10 == 3 && modulo100 != 13) {
      return `${i}rd`;
  }
  return `${i}th`;
}

// Options object to be used with the toLocaleTimeString() method on the now object
const timeOptions = {
  hour: "2-digit",
  minute: "2-digit",
  timeZoneName: 'short'
};

// Build the output string
const str = `It is ${now.toLocaleTimeString("en-us", timeOptions)}, ${nowDay} the ${ordinalSuffix(nowDate)} of ${nowMonth}, ${nowYear}`

// Log output to the console
console.log(str); // "It is 3:13 PM PST, Wednesday the 15th of November, 2017"
```

Here is that code in action!

<p><b id='dateTime'></b></p>

<script>
  const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
  const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday'];

  const now = new Date();
  const nowStr = now.toString();
  const nowDate = now.getDate();
  const nowDay = days[now.getDay()];
  const nowYear = now.getFullYear();
  const nowMonth = months[now.getMonth()];

  function ordinalSuffix(i) {
    const modulo10 = i % 10;
    const modulo100 = i % 100;
    if (modulo10 == 1 && modulo100 != 11) {
        return `${i}st`;
    }
    if (modulo10 == 2 && modulo100 != 12) {
        return `${i}nd`;
    }
    if (modulo10 == 3 && modulo100 != 13) {
        return `${i}rd`;
    }
    return `${i}th`;
  }

  const timeOptions = {
    hour: "2-digit",
    minute: "2-digit",
    timeZoneName: 'short'
  };

  const str = `It is ${now.toLocaleTimeString("en-us", timeOptions)}, ${nowDay} the ${ordinalSuffix(nowDate)} of ${nowMonth}, ${nowYear}`

  const elem = document.getElementById('dateTime');

  elem.innerHTML = str;
</script>


### One more thing

That pesky copyright in the footer thing I mentioned in the beginning. Here is something crazy simple.

```js
const copyrightElem = document.getElementById('copyright');

const now = new Date();
const nowYear = now.getFullYear();
const copyright = `All rights reserved. © ${nowYear - 1} - ${nowYear + 1}`;


copyrightElem.innerHTML = copyright;
```

Here is that code in action

<p><b id='copyright'></b></p>

<script>
  const copyrightElem = document.getElementById('copyright');
  const copyright = `All rights reserved. © ${nowYear - 1} - ${nowYear + 1}`;
  copyrightElem.innerHTML = copyright;
</script>
