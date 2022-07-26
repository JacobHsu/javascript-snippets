# filter

## difference

Calculates the difference between two arrays, without filtering duplicate values.

- Create a `Set` from `b` to get the unique values in `b`.
- Use `Array.prototype.filter()` on `a` to only keep values not contained in `b`, using `Set.prototype.has()`.

```js
const difference = (a, b) => {
  const s = new Set(b);
  return a.filter(x => !s.has(x));
};
```

```js
difference([1, 2, 3, 3], [1, 2, 4]); // [3, 3]
```

## Every nth element

Returns every `nth` element in an array.

- Use `Array.prototype.filter()` to create a new array that contains every `nth` element of a given array.

```js
const everyNth = (arr, nth) => arr.filter((e, i) => i % nth === nth - 1);
// index%2: 0,1,0,1,0,1
// index%3: 0,1,2,0,1,2
```

```js
everyNth([1, 2, 3, 4, 5, 6], 2); // [ 2, 4, 6 ]
everyNth([1, 2, 3, 4, 5, 6], 3); // [ 3, 6 ]
```