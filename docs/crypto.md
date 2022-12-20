# Generate UUID (browser)

Generates a UUID in a browser.

- Use `Crypto.getRandomValues()` to generate a UUID, compliant with [RFC4122](https://www.ietf.org/rfc/rfc4122.txt) version 4.
- Use `Number.prototype.toString()` to convert it to a proper UUID (hexadecimal string).

```js
const UUIDGeneratorBrowser = () =>
  ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c =>
    (
      c ^
      (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
    ).toString(16)
  );
```

```js
UUIDGeneratorBrowser(); // '7982fcfe-5721-4632-bede-6000885be57d'

## Calculate SHA-256 hash (Node.js)

Creates a hash for a value using the [SHA-256](https://en.wikipedia.org/wiki/SHA-2) algorithm.
Returns a promise.

- Use `crypto.createHash()` to create a `Hash` object with the appropriate algorithm.
- Use `hash.update()` to add the data from `val` to the `Hash`, `hash.digest()` to calculate the digest of the data.
- Use `setTimeout()` to prevent blocking on a long operation. Return a `Promise` to give it a familiar interface.

```js
const crypto = require('crypto');

const hashNode = val =>
  new Promise(resolve =>
    setTimeout(
      () => resolve(crypto.createHash('sha256').update(val).digest('hex')),
      0
    )
  );
```

```js
hashNode(JSON.stringify({ a: 'a', b: [1, 2, 3, 4], foo: { c: 'bar' } })).then(
  console.log
);
// '04aa106279f5977f59f9067fa9712afc4aedc6f5862a8defc34552d8c7206393'
```

## Caesar cipher

[凱撒密碼](https://zh.wikipedia.org/zh-tw/凱撒密碼) 
[ASCII](https://zh.wikipedia.org/wiki/ASCII)  

Encrypts or decrypts a given string using the Caesar cipher.

- Use the modulo (`%`) operator and the ternary operator (`?`) to calculate the correct encryption/decryption key.
- Use the spread operator (`...`) and `Array.prototype.map()` to iterate over the letters of the given string.
- Use `String.prototype.charCodeAt()` and `String.fromCharCode()` to convert each letter appropriately, ignoring special characters, spaces etc.
- Use `Array.prototype.join()` to combine all the letters into a string.
- Pass `true` to the last parameter, `decrypt`, to decrypt an encrypted string.

```js
const caesarCipher = (str, shift, decrypt = false) => {
  const s = decrypt ? (26 - shift) % 26 : shift;
  const n = s > 0 ? s : 26 + (s % 26);
  return [...str]
    .map((l, i) => {
      const c = str.charCodeAt(i);
      if (c >= 65 && c <= 90) // ASCII Decimal A 65 Z 90
        return String.fromCharCode(((c - 65 + n) % 26) + 65);
      if (c >= 97 && c <= 122) // ASCII Decimal a 97 z 122
        return String.fromCharCode(((c - 97 + n) % 26) + 97);
      return l;
    })
    .join('');
};
```

```js
caesarCipher('Hello World!', -3); // 'Ebiil Tloia!'
caesarCipher('Ebiil Tloia!', 23, true); // 'Hello World!'
```
