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

## divmod

Returns an array consisting of the quotient and remainder of the given numbers.

- Use `Math.floor()` to get the quotient of the division `x / y`.
- Use the modulo operator (`%`) to get the remainder of the division `x / y`.

```js
const divmod = (x, y) => [Math.floor(x / y), x % y];
```

```js
divmod(8, 3); // [2, 2]
divmod(3, 8); // [0, 3]
divmod(5, 5); // [1, 0]
```

## Km to miles

Converts kilometers to miles.

- Follow the conversion formula `mi = km * 0.621371`.

```js
const kmToMiles = km => km * 0.621371;
```

```js
kmToMiles(8.1) // 5.0331051
```

## euclideanDistance

Calculates the distance between two points in any number of dimensions.

- Use `Object.keys()` and `Array.prototype.map()` to map each coordinate to its difference between the two points.
- Use `Math.hypot()` to calculate the Euclidean distance between the two points.

```js
const euclideanDistance = (a, b) =>
  Math.hypot(...Object.keys(a).map(k => b[k] - a[k]));
```

```js
euclideanDistance([1, 1], [2, 3]); // ~2.2361
euclideanDistance([1, 1, 1], [2, 3, 2]); // ~2.4495
```

