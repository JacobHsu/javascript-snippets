# CSVToArray

Converts a comma-separated values (CSV) string to a 2D array.

- Use `Array.prototype.indexOf()` to find the first newline character (`\n`).
- Use `Array.prototype.slice()` to remove the first row (title row) if `omitFirstRow` is `true`.
- Use `String.prototype.split()` to create a string for each row.
- Use `String.prototype.split()` to separate the values in each row, using the provided `delimiter`.
- Omit the second argument, `delimiter`, to use a default delimiter of `','`.
- Omit the third argument, `omitFirstRow`, to include the first row (title row) of the CSV string.

<details>
<summary>CSVToArray</summary>

```js
const CSVToArray = (data, delimiter = ',', omitFirstRow = false) =>
  data
    .slice(omitFirstRow ? data.indexOf('\n') + 1 : 0)
    .split('\n') // ['a,b', 'c,d']
    .map(v => v.split(delimiter)); // [['a', 'b'], ['c', 'd']]
```
</details>

```js
CSVToArray('a,b\nc,d'); // [['a', 'b'], ['c', 'd']];
CSVToArray('a;b\nc;d', ';'); // [['a', 'b'], ['c', 'd']];
CSVToArray('col1,col2\na,b\nc,d', ',', true); // [['a', 'b'], ['c', 'd']];
```

## CSV to JSON

Converts a comma-separated values (CSV) string to a 2D array of objects.
The first row of the string is used as the title row.

- Use `Array.prototype.indexOf()` to find the first newline character (`\n`).
- Use `Array.prototype.slice()` to remove the first row (title row) and `String.prototype.split()` to separate it into values, using the provided `delimiter`.
- Use `String.prototype.split()` to create a string for each row.
- Use `String.prototype.split()` to separate the values in each row, using the provided `delimiter`.
- Use `Array.prototype.reduce()` to create an object for each row's values, with the keys parsed from the title row.
- Omit the second argument, `delimiter`, to use a default delimiter of `,`.

<details>
<summary>CSVToJSON</summary>

```js
const CSVToJSON = (data, delimiter = ',') => {
  const titles = data.slice(0, data.indexOf('\n')).split(delimiter); // 'col1,col2'
  return data
    .slice(data.indexOf('\n') + 1)
    .split('\n') // ['col1,col2', 'a,b', 'c,d']
    .map(v => {
      const values = v.split(delimiter);
      return titles.reduce(
        (obj, title, index) => ((obj[title] = values[index]), obj),
        {}
      );
    });
};
```

</details>

```js
CSVToJSON('col1,col2\na,b\nc,d');
// [{'col1': 'a', 'col2': 'b'}, {'col1': 'c', 'col2': 'd'}];
CSVToJSON('col1;col2\na;b\nc;d', ';');
// [{'col1': 'a', 'col2': 'b'}, {'col1': 'c', 'col2': 'd'}];
```
