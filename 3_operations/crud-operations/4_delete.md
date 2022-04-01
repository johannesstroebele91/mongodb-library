**Table of Contents**

- [Basics](#basics)
- [Methods](#methods)
  - [deleteOne()](#deleteone)
  - [deleteMany()](#deletemany)
- [Filter argument](#filter-argument)

# Basics

Delete one or multiple documents

- in a collection
- e.g. `db.myCollection.deleteOne(filter, options)`
  - `db` access current database
  - `myCollection`access a collection (gets created if does not exists)
  - `deleteOne(filter, options)` method that gets executed on the collection, with parameters:
    - Filter: narrow down which documents to change
    - Options: configuration

# Methods

The delete methods are `deleteOne()` and `deleteMany()`

## deleteOne()

Delete only the first document that matches the criteria

- syntax `db.products.deleteOne(filter, options)`
- e.g. `db.products.deleteOne({ _id: 1 })`
- e.g. `db.users.deleteOne({name: "Chris"})`

## deleteMany()

Deletes all documents that match the criteria

- syntax `db.products.deleteMany(filter, options)`
- e.g. deletes ALL documents with price 19€ `db.products.deleteMany({ price: 19 })`

**All documents can be deleted by**

- passing an empthy filter `{}` e.g. `db.users.updateMany({})` OR
- dropping the collection e.g. `db.users.drop()`

IMPORTANT:

- if multiple criteria are specified
- all must match
- in order that the element is deleted
- e.g. `db.users.deleteMany({age: {$exists: false, isSporty: true}})`

# Filter argument

_Is the same like for the explanation in the filter for the read methods_
