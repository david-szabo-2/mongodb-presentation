# Introduction to Mongo

Check databases
```
show dbs
```

Switch to new database
```
use demo
```

Check collections
```
show collections
```

Insert documents
```
db.persons.insertMany([
	{ name: { first: "John", last: "Doe" }, birthDate: new Date("1988-10-10"), likes: ["sports", "food"] },
	{ name: { first: "Jane", last: "Doe" }, birthDate: new Date("1983-12-13"), likes: ["dogs"] },
	{ name: { first: "Mary", last: "Smith" }, birthDate: new Date("1996-04-21"), likes: ["sports"] }	
])
```

Check collection content
```
db.persons.find({})
```

Queries
```
db.persons.find({"name.last": "Doe"})

db.persons.find({birthDate: { $gt: ISODate("1990-01-01")} })

db.persons.find({birthDate: { $gt: ISODate("1990-01-01")}, "name.last": "Doe" })
db.persons.find({birthDate: { $lt: ISODate("1990-01-01")}, "name.last": "Doe" })

db.persons.find({"likes": "sports"})

```

Projections
```
db.persons.find({}, { name: 1, birthDate: 1})

db.persons.find({}, { name: 1, birthDate: 1, _id: 0})

db.persons.find({}, { birthDate: 0, _id: 0})
```