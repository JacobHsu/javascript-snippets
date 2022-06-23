# reduce

## reduceRight

```js
const array1 = [[0, 1], [2, 3], [4, 5]];

const result = array1.reduceRight((accumulator, currentValue) => accumulator.concat(currentValue));

console.log(result);
// expected output: Array [4, 5, 2, 3, 0, 1]
```

reverseString

```js
const reverseString = (string) => {
  return string.split("").reduceRight((acc, s) => acc + s)
}
const string = 'fatfish'
console.log(reverseString(string)) // hsiftaf
```

## Count grouped elements

Groups the elements of an array based on the given function and returns the count of elements in each group.

- Use `Array.prototype.map()` to map the values of an array to a function or property name.
- Use `Array.prototype.reduce()` to create an object, where the keys are produced from the mapped results.

```js
const countBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val) => {
    acc[val] = (acc[val] || 0) + 1;
    return acc;
  }, {});
```

```js
countBy([6.1, 4.2, 6.3], Math.floor); // {4: 1, 6: 2}
// [6.1, 4.2, 6.3].map(Math.floor) // (3) [6, 4, 6]
countBy(['one', 'two', 'three'], 'length'); // {3: 2, 5: 1}
// ['one', 'two', 'three'].map( val => val['length']) // (3) [3, 3, 5]
countBy([{ count: 5 }, { count: 10 }, { count: 5 }], x => x.count)
// {5: 2, 10: 1}
```

## Count occurrences

Counts the occurrences of a value in an array.

- Use `Array.prototype.reduce()` to increment a counter each time the specific value is encountered inside the array.

```js
const countOccurrences = (arr, val) =>
  arr.reduce((a, v) => (v === val ? a + 1 : a), 0);
```

```js
countOccurrences([1, 1, 2, 1, 2, 3], 1); // 3

[1,1,2,1].reduce((a, v) => { 
  console.log(a); // 0 0 0 1
  return v === 2 ? a + 1 : a }, 0)
```
