# 時間

## Add days to date

Calculates the date of `n` days from the given date, returning its string representation.

- Use the `Date` constructor to create a `Date` object from the first argument.
- Use `Date.prototype.getDate()` and `Date.prototype.setDate()` to add `n` days to the given date.
- Use `Date.prototype.toISOString()` to return a string in `yyyy-mm-dd` format.

```js
const addDaysToDate = (date, n) => {
  const d = new Date(date);
  d.setDate(d.getDate() + n);
  return d.toISOString().split('T')[0];
};
```

```js
addDaysToDate('2020-10-15', 10); // '2020-10-25'
addDaysToDate('2020-10-15', -10); // '2020-10-05'
```

## Add minutes to date

Calculates the date of `n` minutes from the given date, returning its string representation.

- Use the `Date` constructor to create a `Date` object from the first argument.
- Use `Date.prototype.getTime()` and `Date.prototype.setTime()` to add `n` minutes to the given date.
- Use `Date.prototype.toISOString()`, `String.prototype.split()` and `String.prototype.replace()` to return a string in `yyyy-mm-dd HH:MM:SS` format.

```js
const addMinutesToDate = (date, n) => {
  const d = new Date(date);
  d.setTime(d.getTime() + n * 60000);
  return d.toISOString().split('.')[0].replace('T',' ');
};
```

```js
addMinutesToDate('2020-10-19 12:00:00', 10); // '2020-10-19 12:10:00'
addMinutesToDate('2020-10-19', -10); // '2020-10-18 23:50:00'
```

## Add weekdays to date

Calculates the date after adding the given number of business days.

- Use `Array.from()` to construct an array with `length` equal to the `count` of business days to be added.
- Use `Array.prototype.reduce()` to iterate over the array, starting from `startDate` and incrementing, using `Date.prototype.getDate()` and `Date.prototype.setDate()`.
- If the current `date` is on a weekend, update it again by adding either one day or two days to make it a weekday.
- **NOTE:** Does not take official holidays into account.

```js
const addWeekDays = (startDate, count) =>
  Array.from({ length: count }).reduce(date => {
    date = new Date(date.setDate(date.getDate() + 1));
    if (date.getDay() % 6 === 0)
      date = new Date(date.setDate(date.getDate() + (date.getDay() / 6 + 1)));
    return date;
  }, startDate);
```

```js
addWeekDays(new Date('Oct 09, 2020'), 5); // 'Oct 16, 2020'
addWeekDays(new Date('Oct 12, 2020'), 5); // 'Oct 19, 2020'
```

## 如何獲得兩個日期之間的差異（以天為單位）？

```js
const getDaysDiffBetweenDates = (dateInitial, dateFinal) =>
  (dateFinal - dateInitial) / (1000 * 3600 * 24);

const res = getDaysDiffBetweenDates(
  new Date("2020-05-29"),
  new Date("2020-06-04")
); // 6
console.log(res);
```

```js
const dayDif = (date1, date2) => Math.ceil(Math.abs(date1.getTime() - date2.getTime()) / 86400000)
    
dayDif(new Date("2021-11-3"), new Date("2022-2-1"))  // 90
```

## daysAgo

Calculates the date of `n` days ago from today as a string representation.

- Use the `Date` constructor to get the current date.
- Use `Math.abs()` and `Date.prototype.getDate()` to update the date accordingly and set to the result using `Date.prototype.setDate()`.
- Use `Date.prototype.toISOString()` to return a string in `yyyy-mm-dd` format.

```js
const daysAgo = n => {
  let d = new Date();
  d.setDate(d.getDate() - Math.abs(n));
  return d.toISOString().split('T')[0];
};
```

```js
daysAgo(20); // 2020-09-16 (if current date is 2020-10-06)
```

## 時間戳轉時間

```js
/* 時間戳轉時間 */
function timestampToTime(cjsj) {
  var date = new Date(cjsj); //時間戳為10位需*1000，時間戳為13位的話不需乘1000
  var Y = date.getFullYear() + "-";
  var M =
    (date.getMonth() + 1 < 10
      ? "0" + (date.getMonth() + 1)
      : date.getMonth() + 1) + "-";
  var D =
    (date.getDate() + 1 < 10 ? "0" + date.getDate() : date.getDate()) + " ";
  return Y + M + D;
}

const dateTime = Date.now();
const timestamp = Math.floor(dateTime);
const res = timestampToTime(timestamp);
console.log( timestamp, res ) // 1591867233363 "2020-06-11 "
```

## 查找日期位於一年中的第幾天

```js
const dayOfYear = (date) => Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);

dayOfYear(new Date());  // 306
```

## Convert GMT to local time

```js
// 👇️ can be any valid GMT date/ date+time string
const dateStr = 'Sat, 15 Jan 2021 09:24:00 GMT';

const date = new Date(dateStr);

// 👇️ "Fri Jan 15 2021 11:24:00 GMT+0200 (Eastern European Standard Time)"
console.log(date);

// ✅ get date and time in the visitor's default locale
console.log(date.toLocaleString()); // 👉️ "1/15/2021, 11:24:00 AM"

// ✅ Get LOCAL (visitor's time zone) date and time components
const hours = date.getHours();
console.log(hours); // 👉️ 11

const minutes = date.getMinutes();
console.log(minutes); // 👉️ 24

const seconds = date.getSeconds();
console.log(seconds); // 👉️ 0

```
