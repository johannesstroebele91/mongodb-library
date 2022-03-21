**Table of Contents**

- [Basic](#basic)
- [Important](#important)
- [insertOne()](#insertone)
- [insertMany()](#insertmany)
- [insert()](#insert)
- [mongoimport](#mongoimport)
- [Configuration](#configuration)

# Basic

Create inserts

- one or more JSON objects
- by passing a respective object
- e.g. `insertOne(data, options)`

Parameters are:

- data: object that should be created written as a key-value pair
- options: configuration

If multiple data is inserted

- MongoDB writes the data into the database
- until it
  - succeeds OR
  - a error occurs
    - e.g. a duplicate key
    - e.g. a wrong schema
    - IMPORTANT: all documents until the document with the error are inserted!

# Important

- the passed objects must be
- "" can be omitted for the key but it will be stored in the db as "someKey"
- After the operation is complete,
  - a message will appear if the command was executed successfully
  - `{ acknowledged: true, insertedId: ObjectId("6232f415509d5adfc7a77a0a") }`
  - and an id automatically generated
- an id is automatically generated if none is specified manually `{ _id: "someIdz3482as"}`
  - PS an id key need to be always named like this `_id`
  - PS prevents multiple objects with the same id

# insertOne()

Create one document in a collection

- syntax `insertOne(data, options)`
- e.g. `db.products.insertOne({"name": "Max"})`

# insertMany()

Create one or multiple documents in a collection

- syntax `insertMany(data, options)`
- e.g. `db.products.insertMany( [ { _id: 10, item: "large box"}, { _id: 11, item: "small box"} ] )`

# insert()

Create one or multiple documents in a collection

- is the ancestor of insertOne() and insertMany()
- should not be used anymore
- Examples are similar to insertOne() and insertMany()
  - e.g. `db.products.insert({"name": "Max"})`
  - e.g. `db.products.insert( [ { _id: 10, item: "large box"}, { _id: 11, item: "small box"} ] )`

# mongoimport

Import one or multiple documents in a collection

- syntax `mongoimport -d cars -c carsList --drop --jsonArray`
- TODO

# Configuration

Via the `options` parameter, the insert can be modified:

- `ordered: boolean`: allows to specify wether the client should perform
  - an ordered insert (true is default)
    - false controls that MongoDB inserts all documents and
    - does not abort after the first document with an error
    - (so it practically just skips the incorrect document)
  - e.g. `db.hobbies.insertMany([{_id: "Yoga", name: "Yoga"}, {_id: "Cooking", name: "Cooking"}, {_id: "Hiking", name: "Hiking"}], {ordered: false})`
- writing concern: this options controls the writing: e.g. `db.persons.insertOne({name: "Max"}, {writeConcern: {w: 1, j: undefined}})` (DEFAULT)
  - `w: <value>`: specified that the write operation has
    - propagated to a specified number of mongod instances or to mongod instances with specified tags
    - `w: 1` e.g. `db.persons.insertOne({name: "Max"}, {writeConcern: {w: 1}})` (DEFAULT!)
      - executes the operation with moderate speed and
      - returns information about success or failure
    - `w: 0` e.g. `db.persons.insertOne({name: "Max"}, {writeConcern: {w: 0}})`
      - executes the operation the fastest,
      - BUT returns no information about success or failure
