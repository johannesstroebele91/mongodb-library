**Table of Contents**

- [Basics](#basics)
- [deleteOne()](#deleteone)
- [deleteMany()](#deletemany)

# Basics

Delete one or multiple documents

- in a collection
- e.g. `db.myCollection.deleteOne(filter, options)`
  - `db` access current database
  - `myCollection`access a collection (gets created if does not exists)
  - `deleteOne(filter, options)` method that gets executed on the collection, which parameters:
    - Filter: narrow down which documents to change
    - Options: configuration

# deleteOne()

Delete one document

- syntax `db.products.deleteOne(filter, options)`
- Example: `db.products.deleteOne({ _id: 1 })`

# deleteMany()

Delete one or multiple documents

- syntax `db.products.deleteMany(filter, options)`
- e.g. `db.products.deleteMany({ price: 19 })`
  - deletes ALL documents that have the price 19€
- e.g. `db.products.updateOne(filter, data, options)`
  - deletes all documents due to empty filter `{}`
