# Array.prototype.slice

`slice()` 方法會回傳一個新陣列物件，為原陣列選擇之 begin 至 end（不含 end）部分的淺拷貝（shallow copy）。而原本的陣列將不會被修改。

## First n elements

Gets the first `n` elements of an array.

- Use `Array.prototype.slice()` with a start value of `0` and an end value of `n` to get the first `n` elements of `arr`.

```js
const firstN = (arr, n) => arr.slice(0, n);
```

```js
firstN(['a', 'b', 'c', 'd'], 2); // ['a', 'b']
```

## Execute function for each array element in reverse

Executes a provided function once for each array element, starting from the array's last element.

- Use `Array.prototype.slice()` to clone the given array and `Array.prototype.reverse()` to reverse it.
- Use `Array.prototype.forEach()` to iterate over the reversed array.

```js
const forEachRight = (arr, callback) =>
  arr
    .slice()
    .reverse()
    .forEach(callback);
```

```js
forEachRight([1, 2, 3, 4], val => console.log(val)); // '4', '3', '2', '1'
```

## Drop list elements from the left based on function

Removes elements in an array until the passed function returns `true`.
Returns the remaining elements in the array.

- Loop through the array, using `Array.prototype.slice()` to drop the first element of the array until the value returned from `func` is `true`.
- Return the remaining elements.

```js
const dropWhile = (arr, func) => {
  while (arr.length > 0 && !func(arr[0])) arr = arr.slice(1);
  return arr;
};
```

```js
dropWhile([1, 2, 3, 4], n => n >= 3); // [3, 4]
```

## First n elements

Gets the first `n` elements of an array.

- Use `Array.prototype.slice()` with a start value of `0` and an end value of `n` to get the first `n` elements of `arr`.

```js
const firstN = (arr, n) => arr.slice(0, n);
```

```js
firstN(['a', 'b', 'c', 'd'], 2); // ['a', 'b']
```

## 重複值處理

使用splice

```js
function uniqueArray(arr){
    for(var i = 0; i < arr.length - 1; i++) {        
      for(var j = i + 1;j < arr.length; j++) {            
        if(arr[j] === arr[i]) {                
          arr.splice(j--, 1);            
        }        
      }    
    }    
    return arr;
}
```

優點：不需要使用額外的存儲空間，空間複雜度為 O（1）  
缺點：需要頻繁的內存移動，雙重循環，時間複雜度為 O（N²）

`Object+Array`最省時間，`splice`的方式最耗時（它比較省空間），
`Set+Array`的簡潔方式在數據量大的時候時間將明顯少於需要O（N²）的Array，同樣是O（N²）的splice和Array，Array的時間要小於經常內存移動操作的splice。
