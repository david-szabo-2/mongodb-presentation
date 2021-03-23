# Text search

```js
db.stores.insert(
   [
     { _id: 1, name: "Java Hut", description: "Coffee and cakes" },
     { _id: 2, name: "Burger Buns", description: "Gourmet hamburgers" },
     { _id: 3, name: "Coffee Shop", description: "Just coffee" },
     { _id: 4, name: "Clothes Clothes Clothes", description: "Discount clothing" },
     { _id: 5, name: "Java Shopping", description: "Indonesian goods" }
     { _id: 6, name: "Bishop's", description: "Craft beer" }
   ]
)

db.stores.createIndex( { name: "text", description: "text" } )

db.stores.find( { $text: { $search: "java coffee shop" } } )

db.stores.find( { $text: { $search: "\"coffee shop\"" } } )

db.stores.find( { $text: { $search: "java shop -coffee" } } )

db.stores.find(
   { $text: { $search: "java coffee shop" } },
   { score: { $meta: "textScore" } }
).sort( { score: { $meta: "textScore" } } )
```

