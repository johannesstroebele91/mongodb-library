# Basics

The most important basic commands are:

- SHOW all databases: `show dbs`
- Show stats of the database: `db.stats()` like collections, average object size
- CREATE a new database or SWITCH to it: `use <someName>`
  - the datbase does not get created
  - before data is stored into it
- Delete a database `db.dropDatabase()`
- Delete a collection `db.myCollection.drop()`
  - myCollection needs to be replaced with
  - the name of the collection
