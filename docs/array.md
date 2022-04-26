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
