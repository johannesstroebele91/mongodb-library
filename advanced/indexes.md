# Basics

Indexes enable to

- retrieve faster a small to medium number of documents
- from a collection

If you have queries that

- regularly return the majority or all of your documents,
- using an index is mostly SLOWER
- so it should not be used in these cases

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

Good to know: the `sort()` function is also sped up

# Example

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

# Usage

## How to Create an Index

Use the `createIndex()`

- by passing the respective field
- as an paremeter
- e.g. `db.contacts.createIndex({"dob.age": 1})`
  - value `1`: ascending
  - value `-1`: descending

## How to Delete an Index

Use the `deleteIndex()`

- by passing the created index
- as an paremeter
- e.g. `db.contacts.createIndex({"dob.age": 1})`
  - value `1`: ascending
  - value `-1`: descending

# Measure the performance advantage

The performance of the operation

- which uses an index
- can be measuered by
- `db.someCollection.explain("executionStats").find()`

# Don't use indexes for each field

Indexes lead to a lower performance

- for `insert` OR `update` operations
- e.g. if creating an index for all fields of a collection
- because if a new document is inserted OR updated
- all indexes need to be adjusted
- because the new value needs to be added and
- the list sorted
