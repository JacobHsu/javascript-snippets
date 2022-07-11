# String.prototype.indexOf()

[String.prototype.indexOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

## countSubstrings

Counts the occurrences of a substring in a given string.

- Use `Array.prototype.indexOf()` to look for `searchValue` in `str`.
- Increment a counter if the value is found and update the index, `i`.
- Use a `while` loop that will return as soon as the value returned from `Array.prototype.indexOf()` is `-1`.

```js
const countSubstrings = (str, searchValue) => {
  let count = 0,
    i = 0;
  while (true) {
    const r = str.indexOf(searchValue, i);
    if (r !== -1) [count, i] = [count + 1, r + 1];
    else return count;
  }
};
```

```js
countSubstrings('tiktok tok tok tik tok tik', 'tik'); // 3
countSubstrings('tutut tut tut', 'tut'); // 4
```

## decapitalize

Decapitalizes the first letter of a string.

- Use array destructuring and `String.prototype.toLowerCase()` to decapitalize first letter, `...rest` to get array of characters after first letter and then `Array.prototype.join()` to make it a string again.
- Omit the `upperRest` argument to keep the rest of the string intact, or set it to `true` to convert to uppercase.

```js
const decapitalize = ([first, ...rest], upperRest = false) =>
  first.toLowerCase() +
  (upperRest ? rest.join('').toUpperCase() : rest.join(''));
```

```js
decapitalize('FooBar'); // 'fooBar'
decapitalize('FooBar', true); // 'fOOBAR'
```
