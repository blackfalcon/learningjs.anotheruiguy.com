> {{book.title}} v{{book.version}}

# A brief introduction to JavaScript

> The basic job of a JavaScript engine, when all is said and done, is to take the JavaScript code that a developer writes and convert it to fast, optimized code that can be interpreted by a browser or even embedded into an application. JavaScriptCore, in fact, calls itself an “optimizing virtual machine”.

The JavaScript a developer writes needs an environment that supports JavaScript in order to process the code into commands and execute them. A supporting environment is also called the 'Runtime'. This is the code that runs your code.

The runtime environment JavaScript requires to run your code is typically a web browser (Chrome/Safari/Firefox/EDGE) or it can also be a server environment (Node.js), tools written in C++. Every major browser has it's own JavaScript runtime environment and the current leading JavaScript server is Node.js. See the chart below for a browser, or runtime, and it's supporting JavaScript engine.

| Browser, Headless Browser, or Runtime | JavaScript Engine |
|-|-|
| Mozilla | Spidermonkey |
| Safari †† | JavaScriptCore † |
| Chrome | V8 |
| IE / EDGE | Chakra |
| PhantomJS | JavaScriptCore |
| HTMLUnit | Rhino |
| TrifleJS | V8 |
| Node.js ††† | V8 |

<small style="font-size: 0.8em">† JavaScriptCore was rewritten as SquirrelFish, rebranded as SquirrelFish Extreme also called Nitro. It’s still a true statement however to call JavaScriptCore the JavaScript engine that underlies WebKit implementations (such as Safari).</small>

<small style="font-size: 0.8em">†† iOS developers should be aware that Mobile Safari leverages Nitro, but UIWebView does not include JIT compilation, so the experience is slower. With iOS8, however, developers can use WKWebView which includes access to Nitro, speeding up the experience considerably. Hybrid mobile app developers should be able to breathe a little easier.</small>

<small style="font-size: 0.8em">††† One of the factors in the decision to split io.js from Node.js had to do with the version of V8 that would be supported by the project. This continues to be a challenge.</small>

<small style="font-size: 0.8em">Source: [A Guide to JavaScript Engines for Idiots](https://developer.telerik.com/featured/a-guide-to-javascript-engines-for-idiots/)</small>

## What is ECMAScript?

ECMAScript is becoming a more poplar term, especially with the roll out of ECMAScript 2015, also more commonly known as ES6. In short, ECMAScript is the standard from which JavaScript is written. Per the table above it is understood that there are multiple JavaScript engines in use today, ECMA (European Computer Manufacturers Association) is the international standards organization that oversees the documentation of the JavaScript standard.

> Ecma aims to develop standards and technical reports to facilitate and standardize the use of information communication technology and consumer electronics; encourage the correct use of standards by influencing the environment in which they are applied; and publish these standards and reports in electronic and printed form.

Without this oversight, implementation of JavaScript by different vendors would lead to proprietary features and would cause endless confusion with developers for the emerging web. JavaScript was invented by Netscape and released in March of 1996. August of the same year Microsoft released JScript that featured alternative date models that addressed the growing fear of the Y2K problem.

In turn, Netscape delivered it's version of JavaScript to Ecma International in November of 1996. The name ECMAScript was a compromise between Netscape and Microsoft and the first version was released in June of 1997.

For a full [list of versions](https://en.wikipedia.org/wiki/ECMAScript#Versions) and their history.

For a good listing of ES6 features and the browsers that support them, check out [caniuse.com](https://caniuse.com/#search=ES6)
