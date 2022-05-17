# Array

## 合併兩個數組

```js
const merge = (a, b) => a.concat(b);
const merge = (a, b) => [...a, ...b];
```

## 從數組中移除重復項

`Set` 物件可讓你儲存任何類型的唯一值（unique）

```js
const removeDuplicates = (arr) => [...new Set(arr)];
console.log(removeDuplicates([1, 2, 2, 3, 3, 4, 4, 5, 5, 6])); //  [1, 2, 3, 4, 5, 6]
```

## map

[javascript-puzzlers](https://github.com/xiaoyu2er/blog/issues/1)

Number/parseInt

```js
["1", "2", "3"].map(parseInt)

parseInt('1', 0);
parseInt('2', 1);
parseInt('3', 2);
// [1, NaN, NaN]
```

## reduce

### Average of numbers

Calculates the average of two or more numbers.

- Use `Array.prototype.reduce()` to add each value to an accumulator, initialized with a value of `0`.
- Divide the resulting array by its length.

```js
const average = (...nums) =>
  nums.reduce((acc, val) => acc + val, 0) / nums.length;
```

```js
average(...[1, 2, 3]); // 2
average(1, 2, 3); // 2
```

### Mapped array average

Calculates the average of an array, after mapping each element to a value using the provided function.

- Use `Array.prototype.map()` to map each element to the value returned by `fn`.
- Use `Array.prototype.reduce()` to add each value to an accumulator, initialized with a value of `0`.
- Divide the resulting array by its length.

```js
const averageBy = (arr, fn) =>
  arr
    .map(typeof fn === 'function' ? fn : val => val[fn]) // [4, 2, 8, 6] // { n: 4 } obj['n'] is 4
    .reduce((acc, cur) => acc + cur, 0) / arr.length;
```

```js
averageBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], o => o.n); // 5
averageBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n'); // 5
```

### Cartesian product

Calculates the cartesian product of two arrays.

- Use `Array.prototype.reduce()`, `Array.prototype.map()` and the spread operator (`...`) to generate all possible element pairs from the two arrays.

```js
const cartesianProduct = (a, b) =>
  a.reduce((p, x) => [...p, ...b.map(y => [x, y])], []);
  // [1,2].map(y=>['a',y])  [["a", 1], ["a", 2]]
```

```js
cartesianProduct(['x', 'y'], [1, 2]);
// [['x', 1], ['x', 2], ['y', 1], ['y', 2]]
```


## every

`const all = (arr, fn = Boolean) => arr.every(fn);`

```js
all([4, 2, 3], x => x > 1); // true
all([1, 2, 3]); // true
```

`const allEqual = arr => arr.every(val => val === arr[0]);`

```js
allEqual([1, 2, 3, 4, 5, 6]); // false
allEqual([1, 1, 1, 1]); // true
```

```js
const allEqualBy = (arr, fn) => {
  const eql = fn(arr[0]);
  return arr.every(val => fn(val) === eql);
};
```

```js
allEqualBy([1.1, 1.2, 1.3], Math.round); // true
allEqualBy([1.1, 1.3, 1.6], Math.round); // false
```

## slice

### Split into chunks

Chunks an array into smaller arrays of a specified size.

- Use `Array.from()` to create a new array, that fits the number of chunks that will be produced.
- Use `Array.prototype.slice()` to map each element of the new array to a chunk the length of `size`.
- If the original array can't be split evenly, the final chunk will contain the remaining elements.

```js
const chunk = (arr, size) =>
  Array.from({ length: Math.ceil(arr.length / size) }, (v, i) =>
    arr.slice(i * size, i * size + size)
  );
```

```js
chunk([1, 2, 3, 4, 5], 2); // [[1, 2], [3, 4], [5]]
```

## find

### Argument coalescing factory

Customizes a coalesce function that returns the first argument which is `true` based on the given validator.

- Use `Array.prototype.find()` to return the first argument that returns `true` from the provided argument validation function, `valid`.

```js
const coalesceFactory = valid => (...args) => args.find(valid);
```

```js
const customCoalesce = coalesceFactory(
  v => ![null, undefined, '', NaN].includes(v)
);
customCoalesce(undefined, null, NaN, '', 'Waldo'); // 'Waldo'
```

## isArray


### Cast to array

Casts the provided value as an array if it's not one.

- Use `Array.prototype.isArray()` to determine if `val` is an array and return it as-is or encapsulated in an array accordingly.

```js
const castArray = val => (Array.isArray(val) ? val : [val]);
```

```js
castArray('foo'); // ['foo']
castArray([1]); // [1]
```

## Chain async functions

- Loop through an array of functions containing asynchronous events, calling `next` when each asynchronous event has completed.

```js
const chainAsync = fns => {
  let curr = 0;
  const last = fns[fns.length - 1];
  const next = () => {
    const fn = fns[curr++];
    fn === last ? fn() : fn(next);
  };
  next();
};
```

```js
chainAsync([
  next => {
    console.log('0 seconds');
    setTimeout(next, 1000);
  },
  next => {
    console.log('1 second');
    setTimeout(next, 1000);
  },
  () => {
    console.log('2 second');
  }
]);
```