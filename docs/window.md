# window

## base-64

`window.btoa()`：將 ASCII 字符串或二進制數據轉換成一個 base64 編碼過的字符串,該方法不能直接作用於 Unicode 字符串。
在各瀏覽器中,使用 window.btoa 對 Unicode 字符串進行編碼都會觸發一個字符越界的異常。
`window.atob()` 函數用來解碼一個已經被 base-64 編碼過的數據。你可以使用 window.btoa() 方法來編碼一個可能在傳輸過程中出現問題的數據，並且在接受數據之後，使用 window.atob() 方法來將數據解碼。例如：你可以把 ASCII 裡面數值 0 到 31 的控制字符進行編碼，傳輸和解碼。

前端可以使用這兩個 API 對 URL 路由參數、敏感信息等進行轉碼，防止明文暴露。

```js
let encodedData = window.btoa('Hello, world'); // 編碼
console.log(encodedData); // SGVsbG8sIHdvcmxk
let decodedData = window.atob(encodedData); // 解碼
console.log(decodedData); // Hello, world
let encodeUTF = window.btoa(encodeURIComponent('啊'));
console.log(encodeUTF); // JUU1JTk1JThB
let decodedUTF = decodeURIComponent(atob(encodeUTF));
console.log(decodedUTF); // 啊
```

## window.onerror

當 JS 運行時錯誤發生時，window 會觸發一個 ErrorEvent 接口的 error 事件，並執行 window.onerror()。

在實際的使用過程中，onerror 主要是來捕獲預料之外的錯誤，而 try-catch 則是用來在可預見情況下監控特定的錯誤，兩者結合使用更加高效。

```js
/**
 * @param {String}  message    錯誤信息
 * @param {String}  source    出錯文件
 * @param {Number}  lineno    行號
 * @param {Number}  colno    列號
 * @param {Object}  error  Error對象（對象）
 */
window.onerror = function (message, source, lineno, colno, error) {
  console.log('捕獲到異常：', { message, source, lineno, colno, error });
  // window.onerror 函數只有在返回 true 的時候，異常才不會向上拋出，否則即使是知道異常的發生控制台還是會顯示 Uncaught Error: xxxxx。
  //  return true;
};
```

[`unhandledrejection`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/unhandledrejection_event)：
  當 Promise 被 reject 且沒有 reject 處理器的時候，會觸發 unhandledrejection 事件；這可能發生在 window 下，但也可能發生在 Worker 中。 unhandledrejection 繼承自 PromiseRejectionEvent，而 PromiseRejectionEvent 又繼承自 Event。因此 unhandledrejection 含有 PromiseRejectionEvent 和 Event 的屬性和方法。

總結
  前端組件/項目中，需要有適當的錯誤處理過程，否則出現錯誤，層層上傳，沒有進行捕獲，就會導致頁面掛掉。

## Crypto.getRandomValues()

Math.random是偽隨機的，具體可參考[隨機數的故事](https://zhuanlan.zhihu.com/p/205359984)。

從 V8 裡 Math.random 的實現邏輯來看，每次會一次性產生 128 個隨機數，並放到 cache 裡面，供後續使用，當 128 個使用完了再重新生成一批隨機數。所以 Math.random 的隨機數具有可預測性，這種由算法生成的隨機數也叫偽隨機數。只要種子確定，隨機算法也確定，便能知道下一個隨機數是什麼。

[Crypto.getRandomValues()](https://developer.mozilla.org/zh-CN/docs/Web/API/Crypto/getRandomValues) 方法讓你可以獲取符合密碼學要求的安全的隨機值。傳入參數的數組被隨機值填充（在加密意義上的隨機）。window.crypto.getRandomValue的實現在 Safari，Chrome 和 Opera 瀏覽器上是使用帶有 1024 位種子的ARC4流密碼。

```js
var array = new Uint32Array(10);
window.crypto.getRandomValues(array);

console.log('Your lucky numbers:');
for (var i = 0; i < array.length; i++) {
  console.log(array[i]);
}
```
