# Exercise

## Import test data

Load the postcodes.json data using MongoDBCompass or mongoimport tool.

### MongoDBCompass
Create new database using the `+` sign next to `Databases`. Name the database `exercise` and the collection `postcodes`.
Then on the newly created collection's tab click `ADD DATA` and `Import JSON or CSV file`.

### mongoimport
Execute from commandline *not* mongo shell.
```sh
mongoimport -d exercise postcodes.json
```

### Check the data

When finished using either method, start mongo shell and check that import was successful.
```js
use exercise

show collections

db.postcodes.findOne({})
```

## Exercises

* Add the `{"postal_code" : "38116", "place" : { "name" : "Graceland", "country" :"US", "state" : "Tennessee", "loc" : [ 19.0419, 47.5328 ] } }` document to the collection

* Change the coordinates of the same document to `[-90.02604930000001, 35.0476912]`

* What is the postal code of Graceland/Tennessee? Restrict the query to the postal code only, no other fields.

* How many postal_codes are in Budapest/Hungary?

* Top 5 countries by number of documents in descending order

### Bonus Exercises

* Extend Graceland with a new field `country_postal` that contains the country code and the postal code concatenated with a dash (e.g. HU-1034).
*Hint: you can use an aggregation pipeline as the second argument of update methods*

* Which places (name) are within 20km around longitude -90.02604930000001 and latitude 35.0476912 (Graceland)? The result must be sorted in alphabetical order and each place appear in the result only once (distinct).

## Help

MongoDB reference: https://docs.mongodb.com/manual/reference/
