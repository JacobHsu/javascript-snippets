# Promise

```js
Promise.resolve(
  Promise.resolve(
    Promise.resolve(
      Promise.resolve(
        Promise.resolve(
          Promise.resolve(5)
        )
      )
    )
  )
).then((val) => console.log(val))
```

```js
Promise.resolve()
  .then(() => Promise.resolve())
  .then(() => Promise.resolve())
  .then(() => Promise.resolve())
  .then(() => Promise.resolve())
  .then(() => Promise.resolve(5))
  .then((val) => console.log(val))
```

每次你想在你的承諾流中寫一個then或catch，首先確保你返回promise，然後轉到最外層的promise（如果你一直遵循這個規則，那應該只有一層）並在那裡添加你的then或catch。只要你在返回，你的值就會冒泡到最外層的promise。這就是你應該做的 "then"。
請記住，你不一定要返回一個Promise來使用then。一旦你在一個promise的上下文中，任何返回的值都會通過它冒泡。Promise、number、字符串、函數、對象，等等。

```js
const id = 5
const lotsOAsync = () => fetch('/big-o-list')
  .then((result) => {
    if (result.ok) {
      return result.json().then((list) => {
        const {url: itemURL } = data.items.find((item) => item.id === id)
        return fetch(itemURL).then((result) => {
          if (result.ok) {
            return result.json().then((data) => data.name)
          } else {
            throw new Error(`Couldn't fetch ${itemURL}`)
          }
        })
      })
    } else {
      throw new Error(`Couldn't fetch big-o-list`)
    }
  })
```

```js
const id = 5
const lotsOAsync = () => fetch('/big-o-list')
  .then((result) => result.ok ? result.json() : Promise.reject(`Couldn't fetch big-o-list`))
  .then(({ items }) => items.find((item) => item.id === id))
  .then(({url}) => fetch(url))
  .then((result) => result.ok ? result.json() : Promise.reject(`Couldn't fetch ${result.request.url}`))
  .then((data) => data.name)
```

## Convert function to variadic

Changes a function that accepts an array into a variadic function.

- Given a function, return a closure that collects all inputs into an array-accepting function.

```js
const collectInto = fn => (...args) => fn(args);
```

```js
const Pall = collectInto(Promise.all.bind(Promise));
let p1 = Promise.resolve(1);
let p2 = Promise.resolve(2);
let p3 = new Promise(resolve => setTimeout(resolve, 2000, 3));
Pall(p1, p2, p3).then(console.log); // [1, 2, 3] (after about 2 seconds)
```
