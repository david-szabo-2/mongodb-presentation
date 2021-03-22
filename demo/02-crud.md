# CRUD operations

Use demo database
```js
use demo
```

## Create operations

```js
db.people.insertOne(
	{ name: { first: "Jim", last: "Jones" }, birthDate: new Date("1977-10-10"), likes: ["food", "sports"] }
)

db.people.insertMany([
	{ name: { first: "John", last: "Doe" }, birthDate: new Date("1988-10-10"), likes: ["sports", "food"] },
	{ name: { first: "Jane", last: "Doe" }, birthDate: new Date("1991-12-13"), likes: ["dogs"] },
	{ name: { first: "Mary", last: "Smith" }, birthDate: new Date("1996-04-21"), likes: ["sports"] }	
])
```

## Read operations

Check collection content
```js

db.people.find({})

db.people.find({birthDate: ISODate("1988-10-10")})

db.people.find({birthDate: { $lt: ISODate("1990-01-01")} })

db.people.find({"name.last": "Doe", birthDate: { $lt: ISODate("1990-01-01")}})
```

Documents

```js
db.people.find({"name.last": "Doe"})

db.people.find({ name: { first: "John", last: "Doe" } })

db.people.find({ name: { last: "Doe" } })

db.people.find({ name: { last: "Doe", first: "John" } })
```

Arrays

```js
db.people.find({ likes: "sports" })

db.people.find({ likes: ["sports", "food"]})

db.people.find({ likes: { $all: ["sports", "food"]} })

db.people.find({ likes: { $all: ["sports"]} })

db.people.find({ likes: { $gt: "g", $lt: "n"} })

db.people.find({ likes: { $elemMatch: { $gt: "g", $lt: "n"} } })

db.people.find({ "likes.0": "sports" })

db.people.find({ "likes": { $size: 1 } })
```

Document array

```js
db.people.insertMany([
	{ name: { first: "Michael", last: "Smith" }, birthDate: new Date("1982-10-18"), children: [ { name: "Simon", age: 11}, { name: "Lily", age: 8 } ] },
	{ name: { first: "Mallory", last: "Jones" }, birthDate: new Date("1979-03-04"), children: [ { name: "Agnes", age: 15} ] }
])

db.people.find({"children.name": "Simon"})

db.people.find({"children.age": { $gt: 10} })

db.people.find({"children.0.age": { $lt: 10} })
```

Projections
```js
db.people.find({}, { name: 1, birthDate: 1})

db.people.find({}, { name: 1, birthDate: 1, _id: 0})

db.people.find({}, { birthDate: 0, _id: 0})
```

Existence & null checks
```js
db.people.find({ children: null })

db.people.find({ children: { $exists: false } })
```

Sort & limit
```js
db.people.find({}, { "_id": 0, "name": 1 }).sort({ "name.last": 1 })

db.people.find({}, { "_id": 0, "name": 1 }).sort({ "name.last": 1, "name.first": 1 })

db.people.find({}, { "_id": 0, "name": 1, birthDate: 1 }).sort({ birthDate: -1 })

db.people.find({}, { "_id": 0, "name": 1, birthDate: 1 }).sort({ birthDate: -1 }).skip(1).limit(3)
```

Cursor
```js
var result = db.people.find({ likes: "sports" })

result
```

```js
var result = db.people.find({ likes: "sports" })

while (result.hasNext()) {
   printjson(result.next().name);
}
```

## Update operations


```js
db.people.updateOne(
   { "name.first": "Jane" },
   {
     $set: { "name.last": "Smith" },
     $currentDate: { lastModified: true }
   }
)

db.people.find({ "name.first": "Jane" })

db.people.updateMany(
   { birthDate: { $lt: ISODate("1985-01-01") }},
   {
     $push: { "likes": "cats" },
     $currentDate: { lastModified: true }
   }
)

db.people.find({})
```

## Delete operations

```js
db.people.deleteOne({ "name.last": "Jones"})

db.people.find({})

db.people.deleteMany({ "name.last": "Smith"})

db.people.find({})
```