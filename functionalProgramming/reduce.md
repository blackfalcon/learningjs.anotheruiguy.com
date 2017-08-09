# Reduce function

This was a mind bender!

```
var data = [
  {
    "date": "201601",
    "totalPersonalSavings": 1188,
    "totalPaidSavings": 2221.85,
    "myTotalSavings": 3409.85
  },
  {
    "date": "201602",
    "totalPersonalSavings": 1344,
    "totalPaidSavings": 111.85,
    "myTotalSavings": 1455.85
  },
  {
    "date": "201603",
    "totalPersonalSavings": 10,
    "totalPaidSavings": 20,
    "myTotalSavings": 30
  },
  {
    "date": "201604",
    "totalPersonalSavings": 100,
    "totalPaidSavings": 50,
    "myTotalSavings": 150
  }
];



// initial reduce function that loops through each object in the array
data = data.reduce(function(accu, item) {

    // create function to round up the returned values in the object
    function roundUp(num) {
      return Math.ceil(num * 100) / 100;
    }

    // return a concatenated response to the nested reducer
    return accu.concat({

      // crete new totalSavings object by reducing
      // the accu value from the previous function
      totalSavings: roundUp(accu.reduce(function (innerAccu, innerItem) {

        // returns a new value for totalSavings
        return innerAccu + innerItem.actualTotalSavings; }, item.myTotalSavings)),


      // actualTotalSavings is created from the initial item.totalSavings value
      // it's this value that is used to calculate the next object's totalSavings
      actualTotalSavings: item.myTotalSavings,

      // carry over the key/value pairs that are not mutated
      date: item.date,
      totalPersonalSavings: item.totalPersonalSavings,
      totalPaidSavings: item.totalPaidSavings
    });
}, []);

console.log(data);
```
