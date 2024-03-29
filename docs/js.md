# Currying

柯裡化（Currying），是指把接受多個參數的函數變換成接受一個單一參數（最初函數的第一個參數）的函數，並且返回接受餘下的參數而且返回結果的新函數的技術。

lodash 實現了_.curry函數，_.curry函數接收一個函數作為參數，返回新的柯裡化（curry）函數。調用新的柯裡化函數時，當傳遞的參數個數小於柯裡化函數要求的參數時，返回一個接收剩餘參數的函數，當傳遞的參數達到柯裡化函數要求時，返回結果。那麼，\_.curry函數是如何判斷傳遞的參數是否到達要求的呢？我們不妨先看看下面的例子：

```js
function func(a, b, c) {
  console.log(func.length, arguments.length);
}
func(1); // 3  1
```

`length` 是函數對象的一個屬性值，指該函數有多少個必須要傳入的參數，那些已定義了默認值的參數不算在內，比如 function（x = 0）的 length 是 0。即形參的數量僅包括第一個具有默認值之前的參數個數。

與之對比的是， `arguments.length` 是函數被調用時實際傳參的個數。


實現 lodash [curry](https://lodash.com/docs/4.17.15#curry) 化函數

```js
// 模擬實現 lodash 中的 curry 方法
function curry(func) {
  return function curriedFn(...args) {
    // 判斷實參和形參的個數
    if (args.length < func.length) {
      return function () {
        return curriedFn(...args.concat(Array.from(arguments)));
      };
    }
    return func(...args);
  };
}

function getSum(a, b, c) {
  return a + b + c;
}

const curried = curry(getSum);

console.log(curried(1, 2, 3)); // 6
console.log(curried(1)(2, 3)); // 6
console.log(curried(1, 2)(3)); // 6
```

## 精度

為了避免產生精度差異，我們要把需要計算的數字乘以 10 的 n 次冪，換算成計算機能夠精確識別的整數，然後再除以 10 的 n 次冪，大部分編程語言都是這樣處理精度差異的，我們就借用過來處理一下 JS 中的浮點數精度誤差。

傳入 n 次冪的 n 值：

```js
formatNum = function (f, digit) {
  var m = Math.pow(10, digit);
  return parseInt(f * m, 10) / m;
};
var num1 = 0.1;
var num2 = 0.2;
console.log(num1 + num2);
console.log(formatNum(num1 + num2, 1));
```

動計算 n 次冪的 n 值：

```js
/**
 * 精確加法
 */
function add(num1, num2) {
  const num1Digits = (num1.toString().split('.')[1] || '').length;
  const num2Digits = (num2.toString().split('.')[1] || '').length;
  const baseNum = Math.pow(10, Math.max(num1Digits, num2Digits));
  return (num1 * baseNum + num2 * baseNum) / baseNum;
}
add(0.1, 0.2); // 0.3
```

使用類庫：
  通常這種對精度要求高的計算都應該交給後端去計算和存儲，因為後端有成熟的庫來解決這種計算問題。前端也有幾個不錯的類庫：

[Math.js](https://mathjs.org/)
[decimal.js](http://mikemcl.github.io/decimal.js/)
[big.js](http://mikemcl.github.io/big.js/)