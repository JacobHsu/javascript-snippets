# RegExp

## Case-insensitive substring search

Checks if a string contains a substring, case-insensitive.

- Use the `RegExp` constructor with the `'i'` flag to create a regular expression, that matches the given `searchString`, ignoring the case.
- Use `RegExp.prototype.test()` to check if the string contains the substring.

```js
const includesCaseInsensitive = (str, searchString) =>
  new RegExp(searchString, 'i').test(str);
```

```js
includesCaseInsensitive('Blue Whale', 'blue'); // true
```

## Indent string

Indents each line in the provided string.

- Use `String.prototype.replace()` and a regular expression to add the character specified by `indent` `count` times at the start of each line.
- Omit the third argument, `indent`, to use a default indentation character of `' '`.

jsref_obj_regexp [Modifiers](https://www.w3schools.com/jsref/jsref_obj_regexp.asp)

i : 的意思是不區分大小寫的比對
g : 表示會可比對多個成功的結果，預設沒有 g 時，就是比對到一個就停止
m : 意思是變成每一行文字都有字首與字尾，變成可能每一行文字都是一組字首字尾，

```js
const indentString = (str, count, indent = ' ') =>
  str.replace(/^/gm, indent.repeat(count));
```

```js
indentString('Lorem\nIpsum', 2); // '  Lorem\n  Ipsum'
indentString('Lorem\nIpsum', 2, '_'); // '__Lorem\n__Ipsum'
```

## Check if absolute URL

Checks if the given string is an absolute URL.

- Use `RegExp.prototype.test()` to test if the string is an absolute URL.

```js
const isAbsoluteURL = str => /^[a-z][a-z0-9+.-]*:/.test(str);
```

```js
isAbsoluteURL('https://google.com'); // true
isAbsoluteURL('ftp://www.myserver.net'); // true
isAbsoluteURL('/foo/bar'); // false
```

## String is anagram

Checks if a string is an anagram of another string (case-insensitive, ignores spaces, punctuation and special characters).

- Use `String.prototype.toLowerCase()` and `String.prototype.replace()` with an appropriate regular expression to remove unnecessary characters.
- Use `String.prototype.split()`, `Array.prototype.sort()` and `Array.prototype.join()` on both strings to normalize them, then check if their normalized forms are equal.

```js
const isAnagram = (str1, str2) => {
  const normalize = str =>
    str
      .toLowerCase()
      .replace(/[^a-z0-9]/gi, '')
      .split('')
      .sort()
      .join('');
  return normalize(str1) === normalize(str2);
};
```

```js
isAnagram('iceman', 'cinema'); // true
```
