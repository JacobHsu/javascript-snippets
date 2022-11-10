# Object

## 檢查對像是否為空

檢查對象的空性實際上比看起來要困難得多，即使對象為空，每次檢查對像是否等於 {} 也會返回 false。

`const isEmpty = obj => Reflect.ownKeys(obj).length === 0 && obj.constructor === Object`

## Object.freeze

防止對象被篡改，可以試試 Object.seal 和 Object.freeze


Object.seal 防止新增和刪除屬性
  通常，一個對像是可擴展的（可以添加新的屬性）。Object.seal()方法封閉一個對象會讓這個對象變的不能添加新屬性，且所有已有屬性會變的不可配置。屬性不可配置的效果就是屬性變的不可刪除，以及一個數據屬性不能被重新定義成為訪問器屬性，或者反之。當前屬性的值只要原來是可寫的就可以改變。嘗試刪除一個密封對象的屬性或者將某個密封對象的屬性從數據屬性轉換成訪問器屬性，結果會靜默失敗或拋出 TypeError。

數據屬性包含一個數據值的位置，在這個位置可以讀取和寫入值。訪問器屬性不包含數據值。它包含一對 getter 和 setter 函數。當讀取訪問器屬性時，會調用 getter 函數並返回有效值；當寫入訪問器屬性時，會調用 setter 函數並傳入新值，setter 函數負責處理數據。


```js
const person = {
  name: 'jack',
};
Object.seal(person);
delete person.name;
console.log(person); // {name: "jack"}
```

`Object.freeze` 凍結對象   Object.freeze() 方法可以凍結一個對象。一個被凍結的對象再也不能被修改；凍結了一個對象則不能向這個對象添加新的屬性，不能刪除已有屬性，不能修改該對像已有屬性的可枚舉性、可配置性、可寫性，以及不能修改已有屬性的值。此外，凍結一個對象後該對象的原型也不能被修改。freeze() 返回和傳入的參數相同的對象。

```js
const obj = {
  prop: 42,
};
Object.freeze(obj);
obj.prop = 33;
// Throws an error in strict mode
console.log(obj.prop);
// expected output: 42
```


`Object.freeze`是淺凍結，即只凍結一層，要使對象不可變，需要遞歸凍結每個類型為對象的屬性（深凍結）。使用Object.freeze()凍結的對象中的現有屬性值是不可變的。用Object.seal()密封的對象可以改變其現有屬性值。同時可以使用 Object.isFrozen、Object.isSealed、Object.isExtensible 判斷當前對象的狀態。

[`Object.defineProperty`](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 凍結單個屬性：設置 enumable/writable 為 false，那麼這個屬性將不可遍歷和寫

靜態方法 Object.defineProperty() 會直接對一個物件定義、或是修改現有的屬性。執行後會回傳定義完的物件。

```js
const object1 = {};

Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: false
});

// being explicit
// Object.defineProperty(obj, 'key', {
//   enumerable: false,
//   configurable: false,
//   writable: false,
//   value: 'static'
// });

object1.property1 = 77;
// throws an error in strict mode

console.log(object1.property1);
// expected output: 42
```

## Combine object arrays

Combines two arrays of objects, using the specified key to match objects.

- Use `Array.prototype.reduce()` with an object accumulator to combine all objects in both arrays based on the given `prop`.
- Use `Object.values()` to convert the resulting object to an array and return it.

```js
const combine = (a, b, prop) =>
  Object.values(
    [...a, ...b].reduce((acc, v) => {
      if (v[prop]) // id
        acc[v[prop]] = acc[v[prop]]
          ? { ...acc[v[prop]], ...v }
          : { ...v };
      return acc;
    }, {})
  );
```

```js
const x = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Maria' }
];
const y = [
  { id: 1, age: 28 },
  { id: 3, age: 26 },
  { age: 3}
];
combine(x, y, 'id');
// [
//  { id: 1, name: 'John', age: 28 },
//  { id: 2, name: 'Maria' },
//  { id: 3, age: 26 }
// ]
```

## commonKeys

Finds the common keys between two objects.

- Use `Object.keys()` to get the keys of the first object.
- Use `Object.prototype.hasOwnProperty()` to check if the second object has a key that's in the first object.
- Use `Array.prototype.filter()` to filter out keys that aren't in both objects.

```js
const commonKeys = (obj1, obj2) =>
  Object.keys(obj1).filter(key => obj2.hasOwnProperty(key)); // ["a", "b"].filter
```

```js
commonKeys({ a: 1, b: 2 }, { a: 2, c: 1 }); // ['a']
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
