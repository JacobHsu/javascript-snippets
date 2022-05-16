# Arithmetic progression

Creates an array of numbers in the arithmetic progression, starting with the given positive integer and up to the specified limit.

- Use `Array.from()` to create an array of the desired length, `lim / n`. Use a map function to fill it with the desired values in the given range.

```js
const arithmeticProgression  = (n, lim) =>
  Array.from({ length: Math.ceil(lim / n) }, (_, i) => (i + 1) * n );
```

```js
arithmeticProgression(5, 25); // [5, 10, 15, 20, 25]
arithmeticProgression(3, 15); // [3, 6, 9, 12, 15] 
```

## Closest numeric match

Finds the closest number from an array.

- Use `Array.prototype.reduce()` to scan all elements of the array.
- Use `Math.abs()` to compare each element's distance from the target value, storing the closest match.

```js
const closest = (arr, n) =>
  arr.reduce((acc, num) => (Math.abs(num - n) < Math.abs(acc - n) ? num : acc));
```

```js
closest([6, 1, 3, 7, 9], 5); // 6
``
