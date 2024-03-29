## Get base URL

Gets the current URL without any parameters or fragment identifiers.

- Use `String.prototype.replace()` with an appropriate regular expression to remove everything after either `'?'` or `'#'`, if found.

```js
const getBaseURL = url => url.replace(/[?#].*$/, '');
```

```js
getBaseURL('http://url.com/page?name=Adam&surname=Smith');
// 'http://url.com/page'
```

## Redirect to HTTPS

Redirects the page to HTTPS if it's currently in HTTP.

- Use `location.protocol` to get the protocol currently being used.
- If it's not HTTPS, use `location.replace()` to replace the existing page with the HTTPS version of the page.
- Use `location.href` to get the full address, split it with `String.prototype.split()` and remove the protocol part of the URL.
- Note that pressing the back button doesn't take it back to the HTTP page as its replaced in the history.

```js
const httpsRedirect = () => {
  if (location.protocol !== 'https:')
    location.replace('https://' + location.href.split('//')[1]);
};
```

```js
httpsRedirect();
// If you are on http://mydomain.com, you are redirected to https://mydomain.com
```

## Expand tabs into spaces

Convert tabs to spaces, where each tab corresponds to `count` spaces.

- Use `String.prototype.replace()` with a regular expression and `String.prototype.repeat()` to replace each tab character with `count` spaces.

```js
const expandTabs = (str, count) => str.replace(/\t/g, ' '.repeat(count));
```

```js
expandTabs('\t\tlorem', 3); // '      lorem'
```
