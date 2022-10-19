# prototype.map

## Digitize number

Converts a number to an array of digits, removing its sign if necessary.

- Use `Math.abs()` to strip the number's sign.
- Convert the number to a string, using the spread operator (`...`) to build an array.
- Use `Array.prototype.map()` and `parseInt()` to transform each value to an integer.

```js
const digitize = n => [...`${Math.abs(n)}`].map(i => parseInt(i));
```

```js
digitize(123); // [1, 2, 3]
digitize(-123); // [1, 2, 3]
```

## Juxtapose functions

Takes several functions as argument and returns a function that is the juxtaposition of those functions.

- Use `Array.prototype.map()` to return a `fn` that can take a variable number of `args`.
- When `fn` is called, return an array containing the result of applying each `fn` to the `args`.

```js
const juxt = (...fns) => (...args) => [...fns].map(fn => [...args].map(fn));
```

```js
juxt(
  x => x + 1,
  x => x - 1,
  x => x * 10
)(1, 2, 3); // [[2, 3, 4], [0, 1, 2], [10, 20, 30]]
juxt(
  s => s.length,
  s => s.split(' ').join('-')
)('30 seconds of code'); // [[18], ['30-seconds-of-code']]
```
