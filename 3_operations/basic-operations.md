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

# Get help

Help for the command information can be accessed by

- starting the shell using `mongo` or `mongosh` (also via MongoDB compass) and
- typing in the command line
  - `help` for general information (e.g. `show databases`)
  - `admin` for admin information (e.g. `ls` for listing files)
  - for concrete information about commands for the database and the collection
    - database commands that can be executed on `db`:
      - by switching to a database via e.g. `use test`
      - and executing `db.help()`
      - which diplays e.g. `db.createCollection()`
    - collection commands that can be executed on `db.someCollection`:
      - by switching to a database via e.g. `use test` and
      - executing `db.someCollection.help()`
      - which diplays e.g. `db.test.insertOne()`
