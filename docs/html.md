# HTML

如何禁止網頁復制粘貼

```js
const html = document.querySelector('html');
html.oncopy = () => {
  alert('NO COPY');
  return false;
};
html.onpaste = () => false;
```

如何使用 js 設置/獲取剪貼板內容

```js
//設置剪切板內容
document.addEventListener('copy', () => {
  const clipboardData =
    event.clipboardData || event.originalEvent?.clipboardData;
  clipboardData?.setData('text/plain', '不管複製什麼，都是我！');
  event.preventDefault();
});

//獲取剪切板的內容
document.addEventListener('paste', () => {
  const clipboardData =
    event.clipboardData || event.originalEvent?.clipboardData;
  const text = clipboardData?.getData('text');
  console.log(text);
  event.preventDefault();
});
```

## Copy to clipboard async

Copies a string to the clipboard, returning a promise that resolves when the clipboard's contents have been updated.

- Check if the [Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API) is available. Use an `if` statement to ensure `Navigator`, `Navigator.clipboard` and `Navigator.clipboard.writeText` are truthy.
- Use `Clipboard.writeText()` to write the given value, `str`, to the clipboard.
- Return the result of `Clipboard.writeText()`, which is a promise that resolves when the clipboard's contents have been updated.
- In case that the Clipboard API is not available, use `Promise.reject()` to reject with an appropriate message.
- **Note:** If you need to support older browsers, you might want to use `Document.execCommand()` instead. You can find out more about it in the [copyToClipboard snippet](/js/s/copy-to-clipboard).

```js
const copyToClipboardAsync = str => {
  if (navigator && navigator.clipboard && navigator.clipboard.writeText)
    return navigator.clipboard.writeText(str);
  return Promise.reject('The Clipboard API is not available.');
};
```

```js
copyToClipboardAsync('Lorem ipsum'); // 'Lorem ipsum' copied to clipboard.
```

實現頁面檢測更新並提示


  開發過程中，經常遇到頁面更新、版本發布時，需要告訴使用人員刷新頁面的情況，甚至有些運營、測試人員覺得切換一下菜單再切回去就是更新了 web 頁面資源，有的分不清普通刷新和強刷的區別，所以實現了一個頁面更新檢測功能，頁面更新了自動提示使用人員刷新頁面。

  基本思路為：使用 webpack 配置打包編譯時在 js 文件名裡添加 hash，然後使用 js 向${window.location.origin}/index.html發送請求，解析出 html 文件裡引入的 js 文件名稱 hash，對比當前 js 的 hash 與新版本的 hash 是否一致，不一致則提示用戶更新版本。

```js
// uploadUtils.jsx
import React from 'react';
import axios from 'axios';
import { notification, Button } from 'antd';

// 彈窗是否已展示（可以改用閉包、單例模式等實現，看起來會更有逼格一點）
let uploadNotificationShow = false;

// 關閉notification
const close = () => {
  uploadNotificationShow = false;
};

// 刷新頁面
const onRefresh = (new_hash) => {
  close();
  // 更新localStorage版本號信息
  window.localStorage.setItem('XXXSystemFrontVesion', new_hash);
  // 刷新頁面
  window.location.reload(true);
};

// 展示提示彈窗
const openNotification = (new_hash) => {
  uploadNotificationShow = true;
  const btn = (
    <Button type='primary' size='small' onClick={() => onRefresh(new_hash)}>
      確認更新
    </Button>
  );
  // 這裡不自動執行更新的原因是：
  // 考慮到也許此時用戶正在使用系統甚至填寫一個很長的表單，那你直接刷新了頁面，或許會被掐死的，哈哈
  notification.open({
    message: '版本更新提示',
    description: '檢測到系統當前版本已更新，請刷新後使用。',
    btn,
    // duration為0時，notification不自動關閉
    duration: 0,
    onClose: close,
  });
};

// 獲取hash
export const getHash = () => {
  // 如果提示彈窗已展示，就沒必要執行接下來的檢查邏輯了
  if (!uploadNotificationShow) {
    // 在 js 中請求首頁地址，這樣不會刷新界面，也不會跨域
    axios
      .get(`${window.location.origin}/index.html?time=${new Date().getTime()}`)
      .then((res) => {
        // 匹配index.html文件中引入的js文件是否變化（具體正則，視打包時的設置及文件路徑而定）
        let new_hash = res.data && res.data.match(/\/static\/js\/main.(.*).js/);
        // console.log(res, new_hash);
        new_hash = new_hash ? new_hash[1] : null;
        // 查看本地版本
        let old_hash = localStorage.getItem('XXXSystemFrontVesion');
        if (!old_hash) {
          // 如果本地沒有版本信息（第一次使用系統），則直接執行一次額外的刷新邏輯
          onRefresh(new_hash);
        } else if (new_hash && new_hash != old_hash) {
          // 本地已有版本信息，但是和新版不同：版本更新，彈出提示
          openNotification(new_hash);
        }
      });
  }
};
```

## Adds a class to an HTML element.

- Use `Element.classList` and `DOMTokenList.add()` to add the specified class to the element.

```js
const addClass = (el, className) => el.classList.add(className);
```

```js
addClass(document.querySelector('p'), 'special');
// The paragraph will now have the 'special' class
```

## Escape HTML

Escapes a string for use in HTML.

- Use `String.prototype.replace()` with a regexp that matches the characters that need to be escaped.
- Use the callback function to replace each character instance with its associated escaped character using a dictionary object.

```js
const escapeHTML = str =>
  str.replace(
    /[&<>'"]/g,
    tag =>
      ({
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        "'": '&#39;',
        '"': '&quot;'
      }[tag] || tag)
  );
```

```js
escapeHTML('<a href="#">Me & you</a>');
// '&lt;a href=&quot;#&quot;&gt;Me &amp; you&lt;/a&gt;'
```

## Get all images in element

Fetches all images from within an element and puts them into an array.

- Use `Element.getElementsByTagName()` to get all `<img>` elements inside the provided element.
- Use `Array.prototype.map()` to map every `src` attribute of each `<img>` element.
- If `includeDuplicates` is `false`, create a new `Set` to eliminate duplicates and return it after spreading into an array.
- Omit the second argument, `includeDuplicates`, to discard duplicates by default.

```js
const getImages = (el, includeDuplicates = false) => {
  const images = [...el.getElementsByTagName('img')].map(img =>
    img.getAttribute('src')
  );
  return includeDuplicates ? images : [...new Set(images)];
};
```

```js
getImages(document, true); // ['image1.jpg', 'image2.png', 'image1.png', '...']
getImages(document, false); // ['image1.jpg', 'image2.png', '...']
```

## Hides all the elements specified

- Use the spread operator (`...`) and `Array.prototype.forEach()` to apply `display: none` to each element specified.

```js
const hide = (...el) => [...el].forEach(e => (e.style.display = 'none'));
```

```js
hide(...document.querySelectorAll('img')); // Hides all <img> elements on the page
```
