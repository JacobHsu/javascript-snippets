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
