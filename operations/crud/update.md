# Basic

Changing one or multiple documents

- in a collection
- e.g. `db.products.updateOne(filter, data, options)`
- by passing the parameters:
  - Filter: narrow down which documents to change
  - Data: describing the change
  - Options: configuration

# updateOne()

Update one document

- syntax `db.products.updateOne(filter, data, options)`
- Example: `db.products.updateOne({ _id: 1 }, { $set: { price: 899 } })`
  - `{$set: {price: 899}}` set$ sets the value of an object
  - if it exists or if it does not exist
- Example: `db.flightData.updateOne({_id: ObjectId("62334d4722f77f96878bcb85")}, {$set: {delayed: true}})`
  - using an ObjectId

# updateMany()

Update one or multiple documents

- syntax `db.products.updateMany(filter, data, options)`
- e.g. `db.products.insertMany([ { "_id" : 1, "name" : "xPhone"}, { "_id" : 2, "name" : "xTablet"}, ]) `

# replaceOne()

Replace one document

- syntax `db.products.replaceOne(filter, data, options)`
- e.g. `db.restaurant.replaceOne( { "name" : "Pizza Peter" }, { "_id": 4, "name" : "Pizza Peter", "Borough" : "Manhattan"}, { upsert: true } );`
  - `$set` is not needed here
  - the new object completely replaces the existing object

# update

Update one or multiple documents

- syntax `db.products.update(filter, data, options)`
- e.g. `db.products.update({ price: 29.99 }, { $set: { price: 19.99 } })`
  - `$set` is not needed here
  - the new object completely replaces the existing object
  - PS DEPRECATED with newer mongosh versions
