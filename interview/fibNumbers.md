# Fibonacci numbers

```js
const fib = [0, 1];

for(let i = 2; i <= 10; i++) {
  console.log(`value of i == ${i}`)
  console.log(`value of i - 2 == ${i-2}`)
  console.log(`value of i - 1 == ${i-1}`)

  fib[i] = fib[i - 2] + fib[i - 1];

  console.log(fib);
  console.log(fib[i]);
}
```


Result

```js
"value of i == 2"
"value of i - 2 == 0"
"value of i - 1 == 1"
[0, 1, 1]
1
"value of i == 3"
"value of i - 2 == 1"
"value of i - 1 == 2"
[0, 1, 1, 2]
2
"value of i == 4"
"value of i - 2 == 2"
"value of i - 1 == 3"
[0, 1, 1, 2, 3]
3
"value of i == 5"
"value of i - 2 == 3"
"value of i - 1 == 4"
[0, 1, 1, 2, 3, 5]
5
"value of i == 6"
"value of i - 2 == 4"
"value of i - 1 == 5"
[0, 1, 1, 2, 3, 5, 8]
8
"value of i == 7"
"value of i - 2 == 5"
"value of i - 1 == 6"
[0, 1, 1, 2, 3, 5, 8, 13]
13
"value of i == 8"
"value of i - 2 == 6"
"value of i - 1 == 7"
[0, 1, 1, 2, 3, 5, 8, 13, 21]
21
"value of i == 9"
"value of i - 2 == 7"
"value of i - 1 == 8"
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
34
"value of i == 10"
"value of i - 2 == 8"
"value of i - 1 == 9"
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
55
```
