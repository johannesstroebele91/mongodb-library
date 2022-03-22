**Table of Contents**

- [Basic](#basic)
- [Methods](#methods)
  - [findOne()](#findone)
  - [find()](#find)
- [Filter](#filter)

# Basic

Get one or multiple documents

- in a collection
- e.g. `db.myCollection.find(filter, options)`
  - `db` access current database
  - `myCollection`access a collection (gets created if does not exists)
  - `find(filter, options)` method that gets executed on the collection, which parameters:
    - Filter: narrow down which documents to change
    - Options: configuration

# Methods

_For read the findOne() and find() methods exist:_

## findOne()

Get the first matching document it finds

- syntax `db.myCollection.findOne(filter, options)`
- e.g. `db.myCollection.findOne({ name: 1, contribs: 1 } )`
- gives back a document (not a cursor)

## find()

Get one or more documents

- syntax `db.myCollection.find(filter, options)`
- e.g. `db.myCollection.find()`
- gives a cursor (not a document)

Get **all** documents in a collection by passing

- no parameter als a filter
  - limited e.g. `db.myCollection.find()`
  - without limits e.g. `db.myCollection.find().toArray()`
  - find all documents and modify / print them `db.passengers.find().forEach((passengerData) => {printjson(passengerData)})`

Important:

- all documents are returned by passing no parameter
- a nicer format for an output can be archived via `db.products.find().pretty()`
- not all documents are return
  - MongoDB aborts after a certian amount of objects
  - because MongoDB does not encourage to return like 20 million objects
  - but the command `it` can be used to show more
  - this is because MongoDB does not return
    - an array of objects
    - but a cursor object that enables to cycle through the results
    - the function `.toArray()` will exhaust the cursor object and show everything

# Filter

_Both findOne() and find() methods support all filter possibilities:_

Actually filter the documents in a collection

- match equality against a **single value**: e.g. `flightData.find({intercontinental: true})` OR
- match using a **query selectors and projection operators**:
  1. **query selectors**: locate data using
     - comparison:
     - logical:
     - element:
     - evaluation:
     - array:
     - comments:
     - geospatial e.g. near some location
     - e.g. `$eq`
     - e.g. range filter e.g. greater than `db.flightData.find({distance: {$gt: 10000}})`
  2. **projetion operators**: modify the data presentation
     - `$`
     - `$elemMatch`
     - `$meta`
     - `$slice`
