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

**Additional functions** are possible after the find() method, such as:

- `pretty()` print the data more nicely
- `count` identify how many documents exist e.g. `db.myCollection.find().count()`
- `max()` get the max value
- `min()` get the min value
- `map()` applies a function for each document and returns the new array
- `forEach()` applies a function for each document and returns NOTHING!

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

1. **query selectors**

Documents can be found using these query selectors:

- comparison
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
- logical
  - `or`: documents that match any of the conditions e.g. `db.movies.find({$or: [{"rating.average": {$lt: 5}}, {"rating.average": {$gt: 9.3}}]})`
  - `nor`: documents that DON'T match any of the conditions e.g. `db.movies.find({$nor: [{"rating.average": {$lt: 5}}, {"rating.average": {$gt: 9.3}}]})`
  - `and`: all found documents must match elements of the array
    - IMPORTANT:
      - `db.movies.find({"rating.average": {$gt: 9}, genres: "Drama"})` is in most cases as
      - the same as `db.movies.find({$and: [{"rating.average": {$gt: 9}}, {genres: "Drama"}]})`
    - BUT `$and` solution is more secure
      - because if properties are used multiple times in a filter
      - e.g. `db.movies.find({"rating.average": {$gt: 9}, genres: "Drama"})`
      - ONLY the last mention of the property is used!!!
      - SO in that case `$and` NEEDS to be used
  - `not`: find documents that include not a certain value e.g. `db.movies.find({runtime: {$not: {$eq: 60}}})`
    - is more complex, and can mostly be substituted with easier statements using `$ne`
    - e.g. `db.movies.find({runtime: {$ne: 60}})`
- element
  - `exists`: finds all documents that have an element that exists e.g. `db.users.find({age: {$exists: true}})`
    - IMPORTANT
      - an element is included although it might have the value `null`
      - e.g.`db.users.find({age: {$exists: false, $ne: null}})`
  - `type`: e.g. "string", "double" (e.g. 2.42, 0213), "int" (e.g. 7, 32)
    - finds all documents which element have a certain **type** e.g. `db.users.find({phone: {$type: "string"}})`
    - finds all documents which element have certain **types** e.g. `db.users.find({phone: {$type: ["int", "string"]}})`
- evaluation
  - `regex`: allows to search for text in a key-value pair of a document
    - e.g. normal search like `db.movies.find({summary: "musical"})` only looks for full equality!!
    - regex enables to search for parts of text in a key-value pair e.g. `db.movies.find({summary: {$regex: /musical/}})`
      - a regex expression is always surrounded by `//`
      - and have in between e.g. `/musical/`
  - `expr`: expression find documents where the comparison of two fields inside of the respective document equals true
    - e.g. where the volume is greater than the target for financial data `db.sales.find({$expr: {$gt: ["$volume", "$target"]}})`
    - to access the value of the key-value pairs `"$someKey"` is needed
    - e.g. `db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: 190}}]}})`
    - e.g. `db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: {$subtract: ["$volume", 10]}, else: "$volume"}}]}})`
- array
  - ``:
- comments
  - ``:
- geospatial e.g. near some location
  - ``:

1. **projetion operators**: modify the data presentation

The data presentation can be modified using these projetion operators:

- `$`
- `$elemMatch`
- `$meta`
- `$slice`
