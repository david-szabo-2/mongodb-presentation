# Aggregation pipelines

Use demo database
```js
use demo
```

Initial dataset
```js
db.gameStats.insertMany([
	{ user_id: 10000, date: ISODate("2021-01-05"), minsPlayed: 25,
		games: [ { game_id: 13, minsPlayed: 10 }, { game_id: 53, minsPlayed: 15 } ] },
	{ user_id: 20000, date: ISODate("2021-01-05"), minsPlayed: 90,
		games: [ { game_id: 13, minsPlayed: 90 } ] },
	{ user_id: 10000, date: ISODate("2021-01-06"), minsPlayed: 30,
		games: [ { game_id: 21, minsPlayed: 30 } ] },
	{ user_id: 10000, date: ISODate("2021-01-06"), minsPlayed: 80,
		games: [ { game_id: 21, minsPlayed: 45 }, { game_id: 53, minsPlayed: 15 }, { game_id: 10, minsPlayed: 20 } ] },
	{ user_id: 20000, date: ISODate("2021-01-06"), minsPlayed: 60,
		games: [ { game_id: 13, minsPlayed: 40 }, { game_id: 53, minsPlayed: 20 } ] }
])
```

Sum play time per user (SQL GROUP BY)
```js
db.gameStats.aggregate( [
   {
     $group: {
        _id: "$user_id",
        totalMinsPlayed: { $sum: "$minsPlayed" }
     }
   }
] )
```

Total sum
```js
db.gameStats.aggregate( [
   {
     $group: {
        _id: null,
        totalMinsPlayed: { $sum: "$minsPlayed" }
     }
   }
] )
```

Count entries
```js
db.gameStats.aggregate( [
   {
     $group: {
        _id: null,
        numberOfEntries: { $sum: 1 }
     }
   }
] )


```

Sum play time per user and date
```js
db.gameStats.aggregate( [
   {
     $group: {
        _id: {
           user_id: "$user_id",
           date: "$date"
        },
        total: { $sum: "$minsPlayed" }
     }
   }
] )
```

Sum play time per user and date if over 60 mins (SQL GROUP BY HAVING)
```js
db.gameStats.aggregate( [
   {
     $group: {
        _id: {
           user_id: "$user_id",
           date: "$date"
        },
        total: { $sum: "$minsPlayed" }
     }
   },
   { $match: { total: { $gt: 60 } } }
] )
```

Sum play time per user and date, but only session over 30 mins (SQL GROUP BY with WHERE)
```js
db.gameStats.aggregate( [
   { $match: { minsPlayed: { $gt: 30 } } },
   {
     $group: {
        _id: "$user_id",
        total: { $sum: "$minsPlayed" }
     }
   }
] )
```

Sum play time per user and game
```js
db.gameStats.aggregate( [
   { $unwind: "$games" }
] )


db.gameStats.aggregate( [
   { $unwind: "$games" },
   {
     $group: {
        _id: {
			user_id: "$user_id",
			game_id: "$games.game_id",
		},
        total: { $sum: "$games.minsPlayed" }
     }
   }
] )
```

Top 3 most played games
```js
db.gameStats.aggregate( [
   { $unwind: "$games" },
   {
     $group: {
        _id: {
			game_id: "$games.game_id",
		},
        total: { $sum: "$games.minsPlayed" }
     }
   },
   { $sort: { total: -1 } },
   { $limit: 3 }
] )
```


Simple JOIN
```js
db.users.insertMany([
	{ _id: 10000, user_name: "john" },
	{ _id: 20000, user_name: "jane" },
	{ _id: 30000, user_name: "mary" },
])

db.gameStats.aggregate( [
   {
     $lookup: {
	   from: "users",
	   localField: "user_id",
	   foreignField: "_id",
	   as: "user"
	 }
   }
] )
```

Users total play time with user name
```js
db.gameStats.aggregate( [
   {
     $group: {
        _id: "$user_id",
        total: { $sum: "$minsPlayed" }
     }
   },
   {
     $lookup: {
	   from: "users",
	   localField: "_id",
	   foreignField: "_id",
	   as: "user"
	 }
   },
   { $unwind: "$user" },
   { $project: {
       "user_name": "$user.user_name",
	   "played": "$total",
	   "_id": 0
   }}
] )
```
