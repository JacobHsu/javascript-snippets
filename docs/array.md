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
