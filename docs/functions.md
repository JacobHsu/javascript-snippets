# Logical and for functions

Checks if both of the given functions return `true` for a given set of arguments.

- Use the logical and (`&&`) operator on the result of calling the two functions with the supplied `args`.

```js
const both = (f, g) => (...args) => f(...args) && g(...args);
```

```js
const isEven = num => num % 2 === 0;
const isPositive = num => num > 0;
const isPositiveEven = both(isEven, isPositive);
isPositiveEven(4); // true
isPositiveEven(-2); // false
```

## Call functions with context

Given a key and a set of arguments, call them when given a context.

- Use a closure to call `key` with `args` for the given `context`.

```js
const call = (key, ...args) => context => context[key](...args);
```

```js
Promise.resolve([1, 2, 3])
  .then(call('map', x => 2 * x))
  .then(console.log); // [ 2, 4, 6 ]

Promise.resolve([1, 2, 3])
  .then(call('map', (pre, cur) => pre + cur))
  .then(console.log); // [1, 3, 5]

Promise.resolve([1, 2, 3])
  .then(call('reduce', (pre, cur) => pre + cur))
  .then(console.log); // 6
```

## Capitalize every word


Capitalizes the first letter of every word in a string.

- Use `String.prototype.replace()` to match the first character of each word and `String.prototype.toUpperCase()` to capitalize it.

[`\b`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)
 
```js
const capitalizeEveryWord = str =>
  str.replace(/\b[a-z]/g, char => char.toUpperCase());
```

```js
capitalizeEveryWord('hello world!'); // 'Hello World!'
```

## Compose functions

Performs right-to-left function composition.

- Use `Array.prototype.reduce()` to perform right-to-left function composition.
- The last (rightmost) function can accept one or more arguments; the remaining functions must be unary.

```js
const compose = (...fns) =>
  fns.reduce((f, g) => (...args) => f(g(...args)));
```

```js
const add5 = x => x + 5;
const multiply = (x, y) => x * y;
const multiplyAndAdd5 = compose(
  add5,
  multiply
);
multiplyAndAdd5(5, 2); // 15
```

## Reverse compose functions

Performs left-to-right function composition.

- Use `Array.prototype.reduce()` to perform left-to-right function composition.
- The first (leftmost) function can accept one or more arguments; the remaining functions must be unary.

```js
const composeRight = (...fns) =>
  fns.reduce((f, g) => (...args) => g(f(...args)));
```

```js
const add = (x, y) => x + y;
const square = x => x * x;
const addAndSquare = composeRight(add, square);
addAndSquare(1, 2); // 9
```

## Converge branching functions

Accepts a converging function and a list of branching functions and returns a function that applies each branching function to the arguments and the results of the branching functions are passed as arguments to the converging function.

- Use `Array.prototype.map()` and `Function.prototype.apply()` to apply each function to the given arguments.
- Use the spread operator (`...`) to call `converger` with the results of all other functions.

```js
const converge = (converger, fns) => (...args) =>
  converger(...fns.map(fn => fn.apply(null, args)));
```

```js
const average = converge((a, b) => a / b, [
  arr => arr.reduce((a, v) => a + v, 0),
  arr => arr.length
]);
average([1, 2, 3, 4, 5, 6, 7]); // 4
```
