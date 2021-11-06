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