**Table of Contents**

- [Basic](#basic)
- [Methods](#methods)
  - [findOne()](#findone)
  - [find()](#find)
    - [Cursors](#cursors)
- [Filter argument](#filter-argument)

# Basic

Get one or multiple documents

- in a collection
- e.g. `db.myCollection.find(filter, options)`
  - `db` access current database
  - `myCollection`access a collection (gets created if does not exists)
  - `find(filter, options)` method that gets executed on the collection, with parameters:
    - Filter: narrow down which documents to change
    - Options: configuration

# Methods

_For read the `findOne()` and `find()` methods exist:_

## findOne()

Get only the first document that matches the criteria

- syntax `db.myCollection.findOne(filter, options)`
- e.g. `db.myCollection.findOne({ name: 1, contribs: 1 } )`
- gives back a document (NOT a cursor)
- arrays can be accessed via nested keys e.g. `db.users.findOne({"hobbies.title": "Sports"})`
- access a single field: e.g. `db.movies.findOne().runtime` => Output: 60

## find()

Get all document that matches the criteria

- syntax `db.myCollection.find(filter, options)`
- e.g. `db.myCollection.find()`
- gives a cursor (not a document)
- arrays can be accessed via nested keys e.g. `db.users.find({"hobbies.title": "Sports"})`
- a nicer format for an output can be archived via `db.products.find().pretty()`

Get **all** documents in a collection by passing

- no parameter als a filter
  - limited e.g. `db.myCollection.find()`
  - without limits e.g. `db.myCollection.find().toArray()`
  - find all documents and modify / print them `db.passengers.find().forEach((passengerData) => {printjson(passengerData)})`

### Cursors

Not all documents are return via find()

- because it could potentially yield millions of documents
- therefore MongoDB aborts after a certian amount of objects
  - the command `it` can be used to show more of the request batch
  - and is also good because mostly many documents (e.g. products)
  - are shown on multiple pages

The reason that `it` command is possible is

- because find() does not return an array of objects
- but a cursor object that
  - enables to cycle through the results

In contrast the function `.toArray()`

- will exhaust the cursor object and
- show every document
- so it should be used cautiously

**Functions that can be attached via the cursor** to the find() method, such as:

- `pretty()` print the data more nicely e.g. `db.myCollection.find().pretty()`
- `count()` identify how many documents exist e.g. `db.myCollection.find().count()`
- `max()` get the max value e.g. `db.myCollection.find().pretty()`
- `min()` get the min value
- `map()` applies a function for each document and returns the new array
- `forEach()` applies a function for each document and returns NOTHING!
  - print all docus in a collection via e.g. `db.movies.find().forEach(doc => {printjson(doc)})`
  - also the cursor can be stored as before
    1. `const dataCursor = db.movies.find()`
    2. `dataCursor.forEach(doc => {printjson(job)})`
- `next()` enables to navigate the documents in a collection by:
  1. saving the cursor in a variable `const dataCursor = db.movies.find()`
  2. find the first document of the cursor `dataCursor.next()`
  3. find the next document of the cursor `dataCursor.next()`
  - PS if there are no more docus left (cursor is exhausted),
    - a error is displayed
    - which can be checked manually via e.g. `dataCursor.hasNext()`
- `sort()` sort docus in a collection via
  - using one criteria
    - ascending: value `1` e.g. `db.movies.find().sort({"rating.average": 1})`
    - descending: value `-1` e.g. `db.movies.find().sort({"rating.average": -1})`
  - using multiple criteria e.g. `db.movies.find().sort({"rating.average": -1, runtime: -1})`
- `skip `skip cursor results
  - e.g. skip first 10 results to display items on the second page (pagination)
  - `db.movies.find().sort({"rating.average": -1}).skip(10)`
  - PS MongoDB will automatically sort first, then skip, then limit (your order doesn't matter)
- `limit` limit cursor results
  - e.g. limit the returned results by 2
  - e.g. `db.movies.find().sort({"rating.average": -1}).limit(2)`
  - PS MongoDB will automatically sort first, then skip, then limit (your order doesn't matter)

# Filter argument

_Both findOne() and find() methods support all filter possibilities:_

Actually filter the documents in a collection

- can work with nested documents using "": e.g. `db.movies.findOne({"rating.average": {$gt: 7}})`
- match equality against a **single value**: e.g. `flightData.find({intercontinental: true})` OR
- match using a **query selectors and projection operators**, which are explained in
  - `query-selectors.md` and
  - `read_returnCertainFields.md`
