**Table of Contents**

- [1. Index for one field](#1-index-for-one-field)
- [2. Index for multiple fields (Compound Index)](#2-index-for-multiple-fields-compound-index)
- [3. Index for self-destroying data (TTL Index)](#3-index-for-self-destroying-data-ttl-index)
- [4. Index for an array (Multi Key Index)](#4-index-for-an-array-multi-key-index)
- [5. Text index](#5-text-index)

# 1. Index for one field

Example:

- creating an index `db.contacts.createIndex({"dob.age": 1})`
- using the index `db.contacts.find({"dob.age": {$gt: 60}})`

# 2. Index for multiple fields (Compound Index)

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

# 3. Index for self-destroying data (TTL Index)

Time to live index is for

- data that destroys itself
- after a predefined time
- e.g. clear session of users or shopping cart
- `db.sessions.createIndex({createdAt: 1}, {expireAfterSeconds: 10})`
- PS deletes only data if new data is added or changed

# 4. Index for an array (Multi Key Index)

An array can be indexed which enables to

1. search for a primitive element of an array faster
   - e.g. create an index for an array `db.contacts.createIndex({hobbies: 1})`
   - e.g. search for an specific element in the array using the index `db.contacts.explain("executionStats").find({hobbies: "Sports"})`
2. search for a object element of an array faster
   - e.g. create an index for an array with objects `db.contacts.createIndex({addresses: 1})`
   - e.g. search for an specific object in the array using the index
     - THAT WORKS `db.contacts.explain("executionStats").find({addresses: {street: "Main Street"}})`
     - THAT DOES NOT WORK `db.contacts.explain("executionStats").find({"addresses.street": "Main Street"})`
       - HOWEVER IT WORKS like this
         - e.g. create an index for an array with objects `db.contacts.createIndex({"addresses.street": 1})`
         - e.g. search for an specific object in the array using the index `db.contacts.explain("executionStats").find({"addresses.street": "Main Street"})`

Combinding an index for an array

- with an primitive data index IS possible
- e.g. `db.contacts.createIndex({name: 1, hobbies: 1})`

HOWEVER, combinding an index for an array

- with another array is NOT possible
- e.g. `db.contacts.createIndex({addresses: 1, hobbies: 1})`
- error message `cannot index parallel arrays`
- because MongoDB would have to make
  - a catesian product, so all possible combinations
  - which would be super performance intensive

# 5. Text index

**Text index** is a

- special Multi Key Index
- which turns a text
- into array of single words
- and stores it essentially like an array
- PS `is`, `a`, `to`, or other similar smaller words are NOT stored

A text index can be created by

- assigning the value `"text"` to the targeted key
- `db.someCollection.createIndex({someText: "text"})`

An value in an text index can be searched by

- using the keywords `{$text: {$search: "someWord"}}`
- e.g. `db.products.find({$text: {$search: "awesome"}})`

Important:

- Casing is not important
- Multiple words can be searched by writing them one after another `db.products.find({$text: {$search: "awesome red"}})`
- Specific phrases can be search by
  - escaping the `""` using `"\"firstWord secondWord\""`
  - e.g. `db.products.find({$text: {$search: "\"red book\""}})`
- A targeted key e.g. description
  - does not need to be added
  - because only one text index
  - per collection is possible
  - due to the high performance cost
  - e.g. `db.products.find({$text: {$search: "awesome"}})`
-
