# Basics

CRUD are typical operations executed against a database, which consist of:

- Create
- Read
- Update
- Delete

The most important CRUD operations are:

# CREATE

Create a new document in a collection

- e.g. `db.products.insertOne({"name": "Max"})`
- `db` for referencing the current database
- `products` creates a collection on the fly or uses an existing one
- various functions for collections exists such as:

## insertOne

`db.products.insertOne({"name": "Max"})`

- insertOne is a query command
- which inserts one JSON object
- by enabling to pass the object
- as a key-value pair
  - "" can be omitted for the key
  - but it will be stored in the db as "someKey"

After the operation is complete,

- a message will appear if the command was executed successfully
- `{ acknowledged: true, insertedId: ObjectId("6232f415509d5adfc7a77a0a") }`
- and an id was automatically generated

# READ

Show all documents in a collection: `db.products.find()`

`db.products.find()`: Outputs the data

`db.products.find().pretty()`: Outputs the data in a nicer format
