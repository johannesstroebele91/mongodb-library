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

- can work with nested documents using "": e.g. `db.movies.findOne({"rating.average": {$gt: 7}})`
- match equality against a **single value**: e.g. `flightData.find({intercontinental: true})` OR
- match using a **query selectors and projection operators**:
  1. **query selectors**: locate data using
     - comparison:
       - `eq`: find documents which equal one value certain values, whereby equality differs for primitive data and arrays:
         - primitive data e.g. `db.movies.findOne({runtime: {$eq: 60}})` is the same as `db.movies.findOne({runtime: 60})`
         - arrays
           - contains the element e.g. `db.movies.findOne({genres: {$eq: "Drama"}})` is the same as `db.movies.findOne({genres: "Drama"})`
           - ONLY contains the element e.g. `db.movies.findOne({genres: {$eq: ["Drama"]}})` is the same as `db.movies.findOne({genres: ["Drama"]})`
       - `ne`: find documents which equal NOT one value e.g. `db.movies.findOne({runtime: {$ne: 60}})`
       - `in`: find documents which equal MULTIPLE values e.g. `db.movies.findOne({runtime: {$in: [30, 42]}})`
       - `nin`: find documents which NOT equal MULTIPLE values e.g. `db.movies.findOne({runtime: {$nin: [30, 42]}})`
       - `gt`: greater then e.g. `db.movies.findOne({runtime: {$gt: 60}})`
       - `gte`: greater then OR equals e.g.`db.movies.findOne({runtime: {$gte: 60}})`
       - `lt`: lower then e.g. `db.movies.findOne({runtime: {$lt: 60}})`
       - `lte`: lower then OR equals e.g. `db.movies.findOne({runtime: {$lt: 60}})`
     - logical: (e.g. ``)
     - element: (e.g. ``)
     - evaluation: (e.g. ``)
     - array: (e.g. ``)
     - comments: (e.g. ``)
     - geospatial e.g. near some location (e.g. ``)
     - e.g. `$eq`
     - e.g. range filter e.g. greater than `db.flightData.find({distance: {$gt: 10000}})`
  2. **projetion operators**: modify the data presentation
     - `$`
     - `$elemMatch`
     - `$meta`
     - `$slice`
