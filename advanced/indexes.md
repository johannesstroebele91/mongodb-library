**Table of Contents**

- [1.Basics](#1basics)
- [2. Explanation](#2-explanation)
- [3. Example](#3-example)
- [4. Usage](#4-usage)
  - [4.1. How to Create an Index](#41-how-to-create-an-index)
  - [4.2. How to Delete an Index](#42-how-to-delete-an-index)
- [5. Kinds of indexes](#5-kinds-of-indexes)
- [5.1. Index for one field](#51-index-for-one-field)
  - [5.2. Index for multiple fields (Compound Index)](#52-index-for-multiple-fields-compound-index)

# 1.Basics

Indexes enable to

- retrieve faster a small to medium number of documents
- from a collection
- based on a field that has:
  - numbers: e.g. age, salary, height
  - strings: e.g. names, addresses
  - booleans: should NOT be use because it does not speed of the query

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

The performance of the operation

- e.g. operation with an without an index
- can be measuered by
- `db.someCollection.explain("executionStats").find()`

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

Good to know: the `sort()` function is also sped up

# 3. Example

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

# 4. Usage

## 4.1. How to Create an Index

Use the `createIndex()`

- by passing the respective field
- as an paremeter
- e.g. `db.contacts.createIndex({"dob.age": 1})`
  - value `1`: ascending
  - value `-1`: descending

## 4.2. How to Delete an Index

Use the `deleteIndex()`

- by passing the created index
- as an paremeter
- e.g. `db.contacts.createIndex({"dob.age": 1})`
  - value `1`: ascending
  - value `-1`: descending

# 5. Kinds of indexes

# 5.1. Index for one field

Example:

- creating an index `db.contacts.createIndex({"dob.age": 1})`
- using the index `db.contacts.find({"dob.age": {$gt: 60}})`

## 5.2. Index for multiple fields (Compound Index)

Each entry in the index is

- NOT a single value
- BUT tow combined ones
- SO the order of the fields is
- EXTREMELY important
- e.g. Find all persons who are 35 and male
  1. create an index `db.contacts.createIndex({"dob.age": 1, gender: 1})`
  2. make the query `db.contacts.explain().find({"dob.age": 35, gender: "male"})`
     - PS the order of the passed parameters does NOT matter!!!
     - PS the order of `createIndex()` does matter cause the index can be used to find
       - either age and gender (IXSCAN) OR
       - just age (IXSCAN)
       - BUT not only the right field (does the COLLSCAN)
