# Solution

Add the {"postal_code" : "38116", "place" : { "name" : "Graceland", "country" :"US", "state" : "Tennessee", "loc" : [ 19.0419, 47.5328 ] } } document to the collection
```js
db.postcodes.insertOne(
	{"postal_code" : "38116", "place" : { "name" : "Graceland", "country" :"US", "state" : "Tennessee", "loc" : [ 19.0419, 47.5328 ] } }
)
```

Change the coordinates of the same document to [-90.02604930000001, 35.0476912]
```js
// id will be different each time
db.postcodes.update({_id: ObjectId("605926f52645ae911f934566") }, { $set: { "place.loc": [-90.02604930000001, 35.0476912] } } )
```

What is the postal code of Graceland/Tennessee? Restrict the query to the postal code only, no other fields.
```js
db.postcodes.find({ "place.name": "Graceland", "place.state": "Tennessee" }, { _id: 0, postal_code: 1})
```

How many postal_codes are in Budapest/Hungary?
```js
db.postcodes.find({ "place.name": "Budapest", "place.country": "HU"}).count()

//OR

db.postcodes.aggregate([
	{ $match: { "place.name": "Budapest", "place.country": "HU"} },
	{ $group: {
		_id: null,
		count: { $sum: 1 }
	}}
])
```

Top 5 countries by number of documents in descending order
```js
db.postcodes.aggregate([
	{ $group: {
		_id: "$place.country",
		count: { $sum: 1}
	}},
	{ $sort: { count: -1 } },
	{ $limit: 5 }
])
```

Extend Graceland with a new field `country_postal` that contains the country code and the postal code concatenated with a dash (e.g. HU-1034).
```js
db.postcodes.update(
	{ "place.name": "Graceland", "place.state": "Tennessee" },
	[ { $set: { country_postal: { $concat: [ "$place.country", "-", "$postal_code" ] } } } ]
)
```

Which places (name) are within 20km around longitude -90.02604930000001 and latitude 35.0476912 (Graceland)? The result must be sorted in alphabetical order and each place appear in the result only once (distinct).
```js
db.postcodes.find( {
	"place.loc": { $geoWithin: { $centerSphere: [ [ -90.02604930000001, 35.0476912 ], 20/6378.1 ]}}
}, { _id: 0, "place.name": 1}).sort("place.name": 1).distinct
```

