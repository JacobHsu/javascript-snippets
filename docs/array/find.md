## Find last matching key

Finds the last key that satisfies the provided testing function.
Otherwise `undefined` is returned.

- Use `Object.keys()` to get all the properties of the object.
- Use `Array.prototype.reverse()` to reverse the order and `Array.prototype.find()` to test the provided function for each key-value pair.
- The callback receives three arguments - the value, the key and the object.

```js
const findLastKey = (obj, fn) =>
  Object.keys(obj)
    .reverse()
    .find(key => fn(obj[key], key, obj));
```

```js
findLastKey(
  {
    barney: { age: 36, active: true },
    fred: { age: 40, active: false },
    pebbles: { age: 1, active: true }
  },
  x => x['active']
); // 'pebbles'
```

## Index of all matches

Finds all indexes of `val` in an array.
If `val` never occurs, returns an empty array.

- Use `Array.prototype.reduce()` to loop over elements and store indexes for matching elements.

```js
const indexOfAll = (arr, val) =>
  arr.reduce((acc, el, i) => (el === val ? [...acc, i] : acc), []); // accumulator currentValue  currentIndex 
```

```js
indexOfAll([1, 2, 3, 1, 2, 3], 1); // [0, 3]
indexOfAll([1, 2, 3], 4); // []
```

## Index of substrings

Finds all the indexes of a substring in a given string.

- Use `Array.prototype.indexOf()` to look for `searchValue` in `str`.
- Use `yield` to return the index if the value is found and update the index, `i`.
- Use a `while` loop that will terminate the generator as soon as the value returned from `Array.prototype.indexOf()` is `-1`.

```js
const indexOfSubstrings = function* (str, searchValue) {
  let i = 0;
  while (true) {
    const r = str.indexOf(searchValue, i);
    if (r !== -1) {
      yield r;
      i = r + 1;
    } else return;
  }
};
```

```js
[...indexOfSubstrings('tiktok tok tok tik tok tik', 'tik')]; // [0, 15, 23]
[...indexOfSubstrings('tutut tut tut', 'tut')]; // [0, 2, 6, 10]
[...indexOfSubstrings('hello', 'hi')]; // []
```

## Find first matching key

Finds the first key that satisfies the provided testing function.
Otherwise `undefined` is returned.

- Use `Object.keys()` to get all the properties of the object, `Array.prototype.find()` to test each key-value pair using `fn`.
- The callback receives three arguments - the value, the key and the object.

```js
const findKey = (obj, fn) =>
  Object.keys(obj).find(key => fn(obj[key], key, obj));
  //  Object.keys(obj).find(key => fn(obj[key])); 'barney'
```

```js
findKey(
  {
    barney: { age: 36, active: true },
    fred: { age: 40, active: false },
    pebbles: { age: 1, active: true }
  },
  x => x['active']
); // 'barney'
```