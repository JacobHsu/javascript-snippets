# Set

## Check if arrays have same contents

Checks if two arrays contain the same elements regardless of order.

- Use a `for...of` loop over a `Set` created from the values of both arrays.
- Use `Array.prototype.filter()` to compare the amount of occurrences of each distinct value in both arrays.
- Return `false` if the counts do not match for any element, `true` otherwise.

```js
const haveSameContents = (a, b) => {
  for (const v of new Set([...a, ...b])) // v: 1 2 4
    if (a.filter(e => e === v).length !== b.filter(e => e === v).length) // [ 1 ].length [ 1 ].length
      return false;
  return true;
};
```

```js
haveSameContents([1, 2, 4], [2, 4, 1]); // true
```
