# Introduction to Mongo

## First collection

Check databases
```js
show dbs
```

Switch to new database
```js
use demo
```

Check collections
```js
show collections
```

Insert documents
```js
db.persons.insertMany([
	{ name: { first: "John", last: "Doe" }, birthDate: new Date("1988-10-10"), likes: ["sports", "food"] },
	{ name: { first: "Jane", last: "Doe" }, birthDate: new Date("1991-12-13"), likes: ["dogs"] },
	{ name: { first: "Mary", last: "Smith" }, birthDate: new Date("1996-04-21"), likes: ["sports"] }	
])
```

Check collection content
```js
db.persons.findOne()

db.persons.find({})
```

## Primary key

Explicit key
```js
db.persons.insertOne({ _id: 123, name: { first: "James", last: "Smith" }, birthDate: new Date("1974-06-10"), likes: ["food"] })

db.persons.find({})
```

## Queries

Queries
```js
db.persons.find({birthDate: ISODate("1988-10-10") })

db.persons.find({"name.last": "Doe"})

db.persons.find({"likes": "sports"})

```
