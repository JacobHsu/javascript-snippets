# 時間

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
