**Table of Contents**

- [Basic](#basic)
- [updateOne()](#updateone)
- [updateMany()](#updatemany)
- [replaceOne()](#replaceone)
- [update() (DEPRECATED)](#update-deprecated)
- ["data" operators](#data-operators)
- ["options" operators](#options-operators)

# Basic

Changing one or multiple documents

- in a collection
- e.g. `db.myCollection.updateOne(filter, data, options)`
  - `db` access current database
  - `myCollection`access a collection (gets created if does not exists)
  - `updateOne(filter, data, options)` method that gets executed on the collection, which parameters:
    - Filter: narrow down which documents to change using e.g. query selectors `query-selectors.md`
    - Data: describing the change
    - Options: configuration

# updateOne()

Update the first document of a collection that matches the filter

- syntax `db.products.updateOne(filter, data, options)`
- Example: `db.products.updateOne({ _id: 1 }, { $set: { price: 899 } })`
  - `{$set: {price: 899}}` set$ sets the value of an object
  - if it exists or if it does not exist
- Example: `db.flightData.updateOne({_id: ObjectId("62334d4722f77f96878bcb85")}, {$set: {delayed: true}})`
  - using an ObjectId

# updateMany()

Update all documents of a collection that match the filter

- syntax `db.products.updateMany(filter, data, options)`
- e.g. `db.products.insertMany([ { "_id" : 1, "name" : "xPhone"}, { "_id" : 2, "name" : "xTablet"}, ]) `

# replaceOne()

Replace one document of a collection that match the filter

- syntax `db.products.replaceOne(filter, data, options)`
- e.g. `db.restaurant.replaceOne( { "name" : "Pizza Peter" }, { "_id": 4, "name" : "Pizza Peter", "Borough" : "Manhattan"}, { upsert: true } );`
  - `$set` is not needed here
  - the new object completely replaces the existing object

# update() (DEPRECATED)

Update onr or all documents of a collection that match the filter

- syntax `db.products.update(filter, data, options)`
- e.g. `db.products.update({ price: 29.99 }, { $set: { price: 19.99 } })`
  - `$set` is not needed here
  - the new object completely replaces the existing object
  - PS DEPRECATED with newer mongosh versions

# "data" operators

These operators improve the capabilities to update documents and

- can be **combined**
- e.g. `db.users.updateOne({name: "Manuel"}, {$inc: {age: 1}, $set: {name: "Peter}})`
- but it is not possible to change the same field in the same command (leads to error)
  The most important operators are:

- `set` describes some fields that should be replaced or added to an existing document
  - update one field
    - updateOne()
      - e.g. overwrites if a hobbies array already existed
      - `db.users.updateOne({_id: ObjectId("6239f80df1f853fe180b50cb")}, {$set: {hobbies: [{title: "Sports", frequency: 5}, {title: "Cooking", frequency: 3}, {title: "Hiking", frequency: 1}]}})`
    - updateMany()
      - e.g. all users that match a certain criteria should get a new field
      - `db.users.updateMany({"hobbies.title": "Sports"}, {$set: {isSporty: true}})`
  - update many fields
    - e.g. `db.users.updateOne({_id: ObjectId("6239f80df1f853fe180b50cb")}, {$set: {age: 28, phone: 01528329712}})`
- `inc` increment OR DECREMENT a value, mostly number, by a number (e.g. 1, -3)
  - e.g. increment the age by one every year
  - e.g. `db.users.updateOne({name: "Manuel"}, {$inc: {age: 1}})`
- `min` updates if the old value is higher than the specified value
  - e.g. age is a minimum of 35, so all lower age entries will be raised to 35
  - `db.users.updateOne({name: "Chris"}, {$min: {age: 32}})`
- `max` updates if the old value is lower than the specified value
  - e.g. `db.users.updateOne({name: "Chris"}, {$max: {age: 38}})`
- `mul` multiply the value with the value that is specified
  - e.g. `db.users.updateOne({name: "Chris"}, {$mul: {age: 1.3}})`
- `unset` drop a field based on a condition (value specified does NOT matter)
  - drops a field e.g. `db.users.updateMany({isSporty: true}, {$unset: {phone: ""}})`
  - write null into a field (better if used with schemas) e.g. `db.users.updateMany({isSporty: true}, {$set: {phone: null}})`
- `rename` renaming a field
  - e.g. `db.users.updateMany({}, {$rename: {age: "totalAge"}})`

# "options" operators

- `upsert` if the docu does not exist, it will be created `db.users.updateOne({}, {}, {upsert: true})`
  - the default is "false" and does not need to be stated in the options argument
  - e.g. `db.users.updateOne({name: "Maria"}, {$set: {age: 29, hobbies: [{title: "Good food", frequency: 3}], isSporty: true}}, {upsert: true})`
    - PS even the name is added from the filter
    - altough it is not specified in the data argument via $set
    - because MongoDB is smart
