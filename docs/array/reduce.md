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

## Array to flags object

Converts an array of strings into an object mapping to true.

- Use `Array.prototype.reduce()` to convert the array into an object, where each array value is used as a key whose value is set to `true`.

```js
const flags = arr => arr.reduce((acc, str) => ({...acc, [str]: true }), {});
```

```js
flags(['red', 'green']); // { red: true, green: true }
```

## Inverts the key-value pairs of an object, without mutating it.

Inverts the key-value pairs of an object, without mutating it.

- Use `Object.keys()` and `Array.prototype.reduce()` to invert the key-value pairs of an object and apply the function provided (if any).
- Omit the second argument, `fn`, to get the inverted keys without applying a function to them.
- The corresponding inverted value of each inverted key is an array of keys responsible for generating the inverted value. If a function is supplied, it is applied to each inverted key.

```js
const invertKeyValues = (obj, fn) =>
  Object.keys(obj).reduce((acc, key) => {
    const val = fn ? fn(obj[key]) : obj[key];
    acc[val] = acc[val] || [];
    acc[val].push(key);
    return acc;
  }, {});
```

```js
invertKeyValues({ a: 1, b: 2, c: 1 }); // { 1: [ 'a', 'c' ], 2: [ 'b' ] }
invertKeyValues({ a: 1, b: 2, c: 1 }, value => 'group' + value);
// { group1: [ 'a', 'c' ], group2: [ 'b' ] }
```

## Value frequencies

Creates an object with the unique values of an array as keys and their frequencies as the values.

- Use `Array.prototype.reduce()` to map unique values to an object's keys, adding to existing keys every time the same value is encountered.

```js
const frequencies = arr =>
  arr.reduce((a, v) => {
    a[v] = a[v] ? a[v] + 1 : 1;
    return a;
  }, {});
```

```js
frequencies(['a', 'b', 'a', 'c', 'a', 'a', 'b']); // { a: 4, b: 2, c: 1 }
frequencies([...'ball']); // { b: 1, a: 1, l: 2 }
```

## Get nested object property from path string

Retrieves a set of properties indicated by the given selectors from an object.

- Use `Array.prototype.map()` for each selector, `String.prototype.replace()` to replace square brackets with dots.
- Use `String.prototype.split()` to split each selector.
- Use `Array.prototype.filter()` to remove empty values and `Array.prototype.reduce()` to get the value indicated by each selector.

```js
const get = (from, ...selectors) =>
  [...selectors].map(s =>
    s
      .replace(/\[([^\[\]]*)\]/g, '.$1.')
      .split('.')
      .filter(t => t !== '')
      .reduce((prev, cur) => prev && prev[cur], from)
  );
```

```js
const obj = {
  selector: { to: { val: 'val to select' } },
  target: [1, 2, { a: 'test' }],
};
get(obj, 'selector.to.val', 'target[0]', 'target[2].a');
// ['val to select', 1, 'test']
```

```js
'selector.to.val'.split('.') //  ['selector', 'to', 'val']
['selector', 'to', 'val'].reduce((prev, cur) => prev && prev[cur]), {selector: { to: { val: 'val to select' } }} )
// {selector: {…}} elector: Object >  to: Object > val: "val to select"
```
