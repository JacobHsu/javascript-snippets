# Array.from()

## Initialize array with values

Initializes and fills an array with the specified values.

- Use `Array.from()` to create an array of the desired length, `Array.prototype.fill()` to fill it with the desired values.
- Omit the last argument, `val`, to use a default value of `0`.

```js
const initializeArrayWithValues = (n, val = 0) =>
  Array.from({ length: n }).fill(val);
```

```js
initializeArrayWithValues(5, 2); // [2, 2, 2, 2, 2]
```

## initialize2DArray

Initializes a 2D array of given width and height and value.

- Use `Array.from()` and `Array.prototype.map()` to generate `h` rows where each is a new array of size `w`.
- Use `Array.prototype.fill()` to initialize all items with value `val`.
- Omit the last argument, `val`, to use a default value of `null`.

```js
const initialize2DArray = (w, h, val = null) =>
  Array.from({ length: h }).map(() => Array.from({ length: w }).fill(val));
```

```js
initialize2DArray(2, 2, 0); // [[0, 0], [0, 0]]
```
