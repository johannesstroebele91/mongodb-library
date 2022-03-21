**Table of Contents**

- [Basic](#basic)
- [Important](#important)
- [insertOne()](#insertone)
- [insertMany()](#insertmany)

# Basic

Create inserts

- one or more JSON objects
- by passing a respective object
- e.g. `insertOne(data, options)`

Parameters are:

- data: object that should be created written as a key-value pair
- options: configuration

# Important

- the passed objects must be
- "" can be omitted for the key but it will be stored in the db as "someKey"
- After the operation is complete,
  - a message will appear if the command was executed successfully
  - `{ acknowledged: true, insertedId: ObjectId("6232f415509d5adfc7a77a0a") }`
  - and an id automatically generated
- an id is automatically generated, but can be added manually like this `{ _id: "someIdz3482as"}`
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
