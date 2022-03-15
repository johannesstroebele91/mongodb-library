# Basics

To use these cmds, the MongoDB server and the shell needs to be started

- show all databases: `show dbs`
- create a new db or switch to a db: `use <someName>`
- create a new document in a collection by passing a key-value pair:
  - `db.products.insertOne({"name": "Max"})`
  - `db` mean the current database
  - `products` creates a collection on the fly or uses an existing one
  - `insertOne({"someKey" : "someValue"})`
    - "" can be omitted for the key
    - but it will be stored in the db as "someKey"
- show all documents in a collection: `db.products.find()`
  - `db.products.find().pretty()` outputs the data in a nicer format
