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