# 時間

## 如何獲得兩個日期之間的差異（以天為單位）？

```js
 const getDaysDiffBetweenDates = (dateInitial, dateFinal) =>
   (dateFinal - dateInitial) / (1000 * 3600 * 24);


 const res = getDaysDiffBetweenDates(new Date('2020-05-29'), new Date('2020-06-04')); // 6。
 console.log(res)
```
