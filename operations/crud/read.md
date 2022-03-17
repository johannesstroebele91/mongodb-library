**Table of Contents**

- [Basic](#basic)
- [find()](#find)
- [findOne()](#findone)
- [Projection (return certain fields)](#projection-return-certain-fields)
  - [Relevance](#relevance)
  - [Explanation](#explanation)
  - [Example](#example)

# Basic

Get one or multiple documents

- in a collection
- e.g. `db.products.find(filter, options)`
- by passing the parameters:
  - Filter: narrow down which documents to change
  - Options: configuration

# find()

Get one or multiple documents

- syntax `db.products.find(filter, options)`
- find all documents in a collection limited e.g. `db.products.find()`
- find all documents in a collection without limits e.g. `db.products.find().toArray()`
- find all documents and modify / print them `db.passengers.find().forEach((passengerData) => {printjson(passengerData)})`
- find one or multiple documents in a collection
  - that match the filter criteria
  - e.g. `flightData.find({intercontinental: true})`
- find alle documents where the filter criteria
  - is e.g. greater than something `distance: {$gt: 10000}`
  - e.g. `db.flightData.find({distance: {$gt: 10000}})`

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

# findOne()

Get the first matching document it finds

- syntax `db.products.findOne(filter, options)`
- e.g. `db.bios.findOne({ name: 1, contribs: 1 } )`

# Projection (return certain fields)

## Relevance

Often, applications only need

- a fraction of the data
- from a database

Data in a database

```JSON
{
    "_id": "7148wxhcj",
    "name": "Max",
    "age": 29,
    "job": "instructor",
}
```

Needed data in application

```JSON
{
    "name": "Max",
    "age": 29,
}
```

## Explanation

Projection enables to

- filters out data
- additionally to the filter parameter

Projection enables is

- very good for performance
- because the data transformation
- happens in the server
- and not in the client

## Example

Specify which key-value pairs are included

- the id is by default included (can be omitted using `_id: 0`)
- and all other key-value pairs are omitted by default
- `db.passengers.find({}, {name: 1, _id: 0})`

Returned data:

```BSON
{ _id: ObjectId("623356a022f77f96878bcb99"),
  name: 'Klaus Arber' },
{ _id: ObjectId("623356a022f77f96878bcb9a"),
  name: 'Albert Twostone' `}
```
