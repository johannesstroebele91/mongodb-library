# Basics

CRUD are typical operations executed against a database, which consist of:

- Create
- Read
- Update
- Delete

The most important CRUD operations are:

CREATE: create a new document in a collection `db.products.insertOne({"name": "Max"})`:

- `db` for referencing the current database
- `products` creates a collection on the fly or uses an existing one
- `insertOne({"someKey" : "someValue"})`
  - insertOne is a query command
  - which enables to pass in a JSON object
  - as a key-value pair
    - "" can be omitted for the key
    - but it will be stored in the db as "someKey"

READ: show all documents in a collection: `db.products.find()`

- `db.products.find()` outputs the data in a nicer format
- `db.products.find().pretty()` outputs the data in a nicer format
