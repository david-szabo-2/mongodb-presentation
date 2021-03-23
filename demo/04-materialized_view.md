# Aggregation pipelines

Use demo database
```js
use demo
```

Initial dataset
```js
db.bakesales.insertMany( [
   { date: new ISODate("2018-12-01"), item: "Cake - Chocolate", quantity: 2, amount: new NumberDecimal("60") },
   { date: new ISODate("2018-12-02"), item: "Cake - Peanut Butter", quantity: 5, amount: new NumberDecimal("90") },
   { date: new ISODate("2018-12-02"), item: "Cake - Red Velvet", quantity: 10, amount: new NumberDecimal("200") },
   { date: new ISODate("2018-12-04"), item: "Cookies - Chocolate Chip", quantity: 20, amount: new NumberDecimal("80") },
   { date: new ISODate("2018-12-04"), item: "Cake - Peanut Butter", quantity: 1, amount: new NumberDecimal("16") },
   { date: new ISODate("2018-12-05"), item: "Pie - Key Lime", quantity: 3, amount: new NumberDecimal("60") },
   { date: new ISODate("2019-01-25"), item: "Cake - Chocolate", quantity: 2, amount: new NumberDecimal("60") },
   { date: new ISODate("2019-01-25"), item: "Cake - Peanut Butter", quantity: 1, amount: new NumberDecimal("16") },
   { date: new ISODate("2019-01-26"), item: "Cake - Red Velvet", quantity: 5, amount: new NumberDecimal("100") },
   { date: new ISODate("2019-01-26"), item: "Cookies - Chocolate Chip", quantity: 12, amount: new NumberDecimal("48") },
   { date: new ISODate("2019-01-26"), item: "Cake - Carrot", quantity: 2, amount: new NumberDecimal("36") },
   { date: new ISODate("2019-01-26"), item: "Cake - Red Velvet", quantity: 5, amount: new NumberDecimal("100") },
   { date: new ISODate("2019-01-27"), item: "Pie - Chocolate Cream", quantity: 1, amount: new NumberDecimal("20") },
   { date: new ISODate("2019-01-27"), item: "Cake - Peanut Butter", quantity: 5, amount: new NumberDecimal("80") },
   { date: new ISODate("2019-01-27"), item: "Tarts - Apple", quantity: 3, amount: new NumberDecimal("12") },
   { date: new ISODate("2019-01-27"), item: "Cookies - Chocolate Chip", quantity: 12, amount: new NumberDecimal("48") },
   { date: new ISODate("2019-01-27"), item: "Cake - Carrot", quantity: 5, amount: new NumberDecimal("36") },
   { date: new ISODate("2019-01-27"), item: "Cake - Red Velvet", quantity: 5, amount: new NumberDecimal("100") },
   { date: new ISODate("2019-01-28"), item: "Cookies - Chocolate Chip", quantity: 20, amount: new NumberDecimal("80") },
   { date: new ISODate("2019-01-28"), item: "Pie - Key Lime", quantity: 3, amount: new NumberDecimal("60") },
   { date: new ISODate("2019-01-28"), item: "Cake - Red Velvet", quantity: 5, amount: new NumberDecimal("100") },
] );
```

Refresh function

```js
updateMonthlySales = function(startDate) {
   db.bakesales.aggregate( [
      { $match: { date: { $gte: startDate } } },
      { $group: { _id: { $dateToString: { format: "%Y-%m", date: "$date" } }, sales_quantity: { $sum: "$quantity"}, sales_amount: { $sum: "$amount" } } },
      { $merge: { into: "monthlybakesales", whenMatched: "replace" } }
   ] );
};
```

Initial run

```js
updateMonthlySales(new ISODate("1970-01-01"));

db.monthlybakesales.find().sort( { _id: 1 } )
```

Additional data load

```js
db.bakesales.insertMany( [
   { date: new ISODate("2019-01-28"), item: "Cake - Chocolate", quantity: 3, amount: new NumberDecimal("90") },
   { date: new ISODate("2019-01-28"), item: "Cake - Peanut Butter", quantity: 2, amount: new NumberDecimal("32") },
   { date: new ISODate("2019-01-30"), item: "Cake - Red Velvet", quantity: 1, amount: new NumberDecimal("20") },
   { date: new ISODate("2019-01-30"), item: "Cookies - Chocolate Chip", quantity: 6, amount: new NumberDecimal("24") },
   { date: new ISODate("2019-01-31"), item: "Pie - Key Lime", quantity: 2, amount: new NumberDecimal("40") },
   { date: new ISODate("2019-01-31"), item: "Pie - Banana Cream", quantity: 2, amount: new NumberDecimal("40") },
   { date: new ISODate("2019-02-01"), item: "Cake - Red Velvet", quantity: 5, amount: new NumberDecimal("100") },
   { date: new ISODate("2019-02-01"), item: "Tarts - Apple", quantity: 2, amount: new NumberDecimal("8") },
   { date: new ISODate("2019-02-02"), item: "Cake - Chocolate", quantity: 2, amount: new NumberDecimal("60") },
   { date: new ISODate("2019-02-02"), item: "Cake - Peanut Butter", quantity: 1, amount: new NumberDecimal("16") },
   { date: new ISODate("2019-02-03"), item: "Cake - Red Velvet", quantity: 5, amount: new NumberDecimal("100") }
] )

updateMonthlySales(new ISODate("2019-01-01"));

db.monthlybakesales.find().sort( { _id: 1 } )
```
