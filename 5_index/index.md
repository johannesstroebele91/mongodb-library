**Table of Contents**

- [1.Basics](#1basics)
- [2. Use cases](#2-use-cases)
- [2. Explanation](#2-explanation)
- [3. Find docus with an index](#3-find-docus-with-an-index)
- [4. Create an Index](#4-create-an-index)
- [5. Delete an Index](#5-delete-an-index)
- [6. Get all existing indexes](#6-get-all-existing-indexes)

# 1.Basics

Indexes enable to

- retrieve faster a small to medium number of documents
- from a collection
- based on a field that has:
  - numbers: e.g. age, salary, height
  - strings: e.g. names, addresses
  - booleans: should NOT be use because it does not speed of the query

# 2. Use cases

Don't use indexes if you have queries that

- return values with
- booleans (true, false) OR
- only two or three values (e.g. gender, priority)
- because the performance is slower

Don't use indexes if you have queries that

- regularly return the majority or all of your documents,
- using an index is mostly SLOWER
- so it should not be used in these cases

Don't use indexes for each field of a collection

- because they lead to a lower performance for `insert` OR `update` operations
- e.g. if creating an index for all fields of a collection
- because if a new document is inserted OR updated
- all indexes need to be adjusted
- because the new value needs to be added and
- the list sorted

Use indexes if `sort()` function leads to an timeout

- due to it taking to long
- e.g. `db.contacts.explain().find({"dob.age": 35}).sort({gender: 1})`

Use the default index `_id` sort documents by the **stanard order**

If you only need the value of an index

- you can directly use the value
- without having to examine any documents
- which is called a **covered query**
- An index covers a query when all of the following apply:
  - all the fields in the query are part of an index, and
  - all the fields returned in the results are in the same index.
  - no fields in the query are equal to null (i.e. {"field" : null} or {"field" : {$eq : null}} ).
- e.g. `db.customers.find({name: "Max"}, {_id: 0, name: 1})`
  - this query is not covered!!! e.g. `db.customers.find({name: "Max"})`

# 2. Explanation

An index can be thought of as an

- simple list of values + pointers
- to the original document
- e.g. `(29, "address in memory/ collection a1")`
  - The documents in the collection would be at the "addresses" a1, a2 and a3
  - The order does not have to match the order in the index (and most likely, it indeed won't).

The important thing is that the index items are **ordered**

- depending on how you created the index
- e.g. `createIndex({age: 1})`
  - value `1`: ascending
  - value `-1`: descending

The performance of the operation

- e.g. operation with an without an index
- can be measuered by
- `db.someCollection.explain("executionStats").find()`

# 3. Find docus with an index

`db.products.find({seller: "Max"})`

If there is `no index` for the searched field e.g. `seller`

- MongoDB does a **COLLSCAN** (collection scan) by
- going through the complete collection and
- each document and
- look for if `seller` equals `Max`

If there is an index

- MongoDB does a **IXSCAN** (index scan) by
- recognizing that for the key `seller` an index exists and
- going to the `seller index`
- which is easier to scan for
- because it is always an ordered list of all values and
- it can directly "jump" to the filtered documents
- e.g. `Anna, Anna, Chris, Manuel, Max, Max`

# 4. Create an Index

Use the `createIndex()`

- by passing the respective field
- as an paremeter
- e.g. `db.contacts.createIndex({"dob.age": 1})`
  - value `1`: ascending
  - value `-1`: descending

Indexes can be created

- in the foreground (normal) OR
- in the background

If an index is create in the foreground

- the collection IS LOCKED
- during that time
- but it is faster
- e.g. `db.ratings.createIndex({age: 1})`
  - which can be enforced via the options
  - e.g. `db.ratings.createIndex({age: 1}, {background: false})`
- IMPLICATION: don't use it on production

If an index is create in the background

- the collection is accessible
- during that time
- but it is slower
- e.g. `db.ratings.createIndex({age: 1}, {background: true})`
- IMPLICATION: useful for production environments

# 5. Delete an Index

Use the `deleteIndex()` to the delete an index by either

1. passing the created index
   - as an paremeter
   - e.g. `db.contacts.createIndex({"dob.age": 1})`
     - value `1`: ascending
     - value `-1`: descending
2. Getting the index name and deleting it via the name
   - get all index names e.g. `db.contacts.getIndexes()`
   - delete the index via its name e.g. `db.contacts.dropIndex("someIndexName"}`

# 6. Get all existing indexes

All existing indexes can be seen using

- `db.myCollection.getIndexes()`
- whereby the default index is `_id`
