# array to object

## Creates an object from an array

Creates an object from an array, using a function to map each value to a key.

- Use `Array.prototype.reduce()` to create an object from `arr`.
- Apply `fn` to each value of `arr` to produce a key and add the key-value pair to the object.

Array.prototype.reduce()  
`accumulator` `currentValue` `currentIndex` `array`  


```js
const indexBy = (arr, fn) =>
  arr.reduce((obj, v, i) => {
    obj[fn(v, i, arr)] = v;
    return obj;
  }, {});
```

```js
indexBy([
  { id: 10, name: 'apple' },
  { id: 20, name: 'orange' }
], x => x.id);
// { '10': { id: 10, name: 'apple' }, '20': { id: 20, name: 'orange' } }
```

## Array to object based on key

Creates an object from an array, using the specified key and excluding it from each value.

- Use `Array.prototype.reduce()` to create an object from `arr`.
- Use object destructuring to get the value of the given `key` and the associated `data` and add the key-value pair to the object.

```js
const indexOn = (arr, key) =>
  arr.reduce((obj, v) => {
    const { [key]: id, ...data } = v;
    obj[id] = data;
    return obj;
  }, {});
```

```js
indexOn([
  { id: 10, name: 'apple' },
  { id: 20, name: 'orange' }
], 'id');
// { '10': { name: 'apple' }, '20': { name: 'orange' } }
```

```js
    const symbols = data.reduce((obj, v, index) => {
      let { poolName } = v;
      obj[index] = poolName;
      return obj;
    }, {});

    // const arrsymbols = Object.values(symbols); [poolName1, poolName2]
    // symbols.includes(symbol)
```
