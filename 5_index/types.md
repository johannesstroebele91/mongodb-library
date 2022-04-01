**Table of Contents**

- [1. Index for one field](#1-index-for-one-field)
- [2. Unique index](#2-unique-index)
- [8. Partial indexes](#8-partial-indexes)
- [2. Index for multiple fields (Compound Index)](#2-index-for-multiple-fields-compound-index)
- [3. Index for self-destroying data (TTL Index)](#3-index-for-self-destroying-data-ttl-index)
- [4. Index for an array (Multi Key Index)](#4-index-for-an-array-multi-key-index)
- [5. Text index](#5-text-index)

# 1. Index for one field

Example:

- creating an index `db.contacts.createIndex({"dob.age": 1})`
- using the index `db.contacts.find({"dob.age": {$gt: 60}})`

# 2. Unique index

Indexes can be configured

- using the second parameter
- for e.g. adding an own unique indexes e.g. email
- e.g. `db.contacts.createIndex({email: 1, {unique: true}})`
  - which checks if the values are unique
  - which are a must

MongoDB treats non-existing values

- still as values in an index
- so cannot insert two documents with no value
- that belong to the unique index
- PS if you want to create such an index, you need to set a `partialFilterExpression`
  - e.g. `db.users.createIndex({email: 1}, {unique: true, partialFilterExpression: {email: {$exists: true}}})`
  - so all of the not existing values are sorted out in advance

# 8. Partial indexes

Partial filters provide a solution to the

- problem that many values in index exist
- that you most of the times don't need
- e.g. always only need people that are over 65 for a retirement application
  - create index `db.contacts.createIndex({"dob.age": 1}, {partialFilterExpression: {gender: "male"}})`
  - get data does NOT work!!!!! `db.contacts.find({"dob.age": {$gt: 60}})`
  - get data `db.contacts.find({"dob.age": {$gt: 60}, gender: "male"})`

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
- whereby `is`, `a`, `to`,
  - or other similar smaller words
  - are NOT stored

**ONLY ONE text index per collection is possible**

- because of the high performance cost
- therefore the targeted key e.g. description
- does not need to be added
- e.g. `db.products.find({$text: {$search: "awesome"}})`
- HOWEVER, two text indexes can be merged by
  1. firstly deleting the text index if it exists by
     - finding out the name of the index `db.someCollection.getIndexes()`AND
     - dropping it via like this e.g. `db.products.dropIndex("description_text")`
  2. Merging multiple text fields
     - which creates one index with all the words from both fields
     - e.g. `db.products.createIndex({title: "text", description: "text"})`
     - Different weights relative to each othe
       - can be assigned for each field
       - using options `weights: {someField: 1, anotherField: 3}`
         1. create an index using weights
            - e.g. description is 10 times more important than description
            - `db.products.createIndex({title: "text", description: "text"}, {weights: {title: 1, description: 10}})`
         2. find a document using weights `db.products.find({$text: {$search: "red"}}, {score: {$meta: "textScore"}})`

A text index can be created by

- assigning the value `"text"` to the targeted key
- `db.someCollection.createIndex({someText: "text"})`

An value in an text index can be searched by

- using the keywords `{$text: {$search: "someWord"}}`
- e.g. `db.products.find({$text: {$search: "awesome"}})`

**Important**

- Casing is by default false
  - but can be set to true
  - e.g. `db.products.find({$text: {$search: "red", $caseSensitive: true}})`
- Multiple words can be searched by writing them one after another `db.products.find({$text: {$search: "awesome red"}})`
- Specific phrases can be search by
  - escaping the `""` using `"\"firstWord secondWord\""`
  - e.g. `db.products.find({$text: {$search: "\"red book\""}})`
- Get the result first for the hits that where more words match by
  - adding a text score via the options `{score: {$meta: "textScore"}`
  - which also sorts the results by the highest score automatically
  - e.g. `db.products.find({$text: {$search: "awesome t-shirt"}}, {score: {$meta: "textScore"}})`
  - sorting can also enforced (not necessary) `db.products.find({$text: {$search: "awesome t-shirt"}}, {score: {$meta: "textScore"}}).sort({score: {$meta: "textScore"}})`
- Exclude words by
  - putting `-` before each word that should be excluded
  - e.g. `db.products.find({$text: {$search: "awesome -t-shirt"}})`
- Defining which stop words and prefixed are removed from the list of words by
  - setting the default language
  - which is by default english
  - e.g. `db.products.createIndex({title: "text", description: "text"}, {default_language: "english"})`
  - list with languages: https://www.mongodb.com/docs/manual/tutorial/specify-language-for-text-index/
