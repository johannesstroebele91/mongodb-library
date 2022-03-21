**Table of Contents**

- [Basics](#basics)
- [Types](#types)
- [1. Nested documents](#1-nested-documents)
- [2. References (like SQL)](#2-references-like-sql)
  - [2.2 Add document to patients collection](#22-add-document-to-patients-collection)
  - [2.2. Add document to diseaseSummaries collection](#22-add-document-to-diseasesummaries-collection)
  - [2.3. Return the id of the diseaseSummary of the patient](#23-return-the-id-of-the-diseasesummary-of-the-patient)
- [Relation types](#relation-types)

# Basics

Multiple collections

- can be related
- which can be done using:
  - nested documents
  - referencing documents

Schemas define

- the relation
- between documents in collections

# Types

There are two options how the relate documents in collections:

1. nested documents
2. references

Relations can be:

- **1:1 relation: embedded objects mostly the best solution** (e.g. each student has one student id)
- **1:n (1: many) relation: embedded objects mostly the best solution** (e.g. one student can attend multiple courses)
  - sometimes also reference
  - e.g. citizens and cities, because many citizens might live in one city
- **n:m relation: mostly with references by either using**
  1. by embedding the id of the e.g. product in the customer OR
  2. make a join table that has both ids (e.g. customers and products) (SQL approach)
  3. by embedding the whole e.g. product in the customer (might lead to duplication and difficult updates)

# 1. Nested documents

Nested documents

- group data together logically
- great for data that
  - belongs together and
  - is not really overlapping with other data
- BUT you should avoid super deep nested data or large arrays

- Pro: only one query needed to fetch all the user data
- Contra: might lead to lots of duplicated code, which is harder to handle if it changes

Add document to the patients collection with the diseaseSummary data

`db.patients.insertOne({name: "Max", age: 29, diseaseSummary: {diseases: ["cold", "broken leg"]}})`

``

# 2. References (like SQL)

Links like the ObjectId

- can be used in collections
- to reference the document in one collection
- to a document in another one

Reference approach is about splitting data across collections, which is

- great for related but shared data as well as
- for data which is used in relations and standalone
- allows you to overcome nesting and size limits (by creating new documents)

Links like the ObjectId

- can be used in collections
- to reference the document in one collection
- to a document in another one

- Pro: easier to handle if the data in one of the collection changes
- Contra: multiple queries needed to fetch all user data

## 2.2 Add document to patients collection

`db.patients.insertOne({name: "Max", age: 29, diseaseSummary: "summary-max-1"})`

Output:

```BSON
{ _id: ObjectId("62348a4a679fb2f726d1ccad"),
  name: 'Max',
  age: 29,
  diseaseSummary: { diseases: [ 'cold', 'broken leg' ] } }
```

## 2.2. Add document to diseaseSummaries collection

`db.diseaseSummaries.insertOne({_id: "summary-max-1", diseases: ["cold", "broken leg"]})`

Output:

```json
{ "_id": "summary-max-1", "diseases": ["cold", "broken leg"] }
```

## 2.3. Return the id of the diseaseSummary of the patient

`db.diseaseSummaries.findOne({_id: db.patients.findOne().diseaseSummary})`

Output: `{ _id: 'summary-max-1', diseases: [ 'cold', 'broken leg' ] }`

In JS you can of course use variables

- if you want to store the data
- in case the query gets to complicated

1. `var dsid = db.patiens.findOne().diseaseSummary`
2. `db.diseaseSummaries.findOne({_id: dsid})`

Output:

```BSON
{ _id: ObjectId("62348a4a679fb2f726d1ccad"),
  name: 'Max',
  age: 29,
  diseaseSummary: { diseases: [ 'cold', 'broken leg' ] } }
```

# Relation types

- 1:1 relation: embedded objects mostly the best solution (e.g. each student has one student id)
- 1:n (1: many) relation: embedded objects mostly the best solution (e.g. one student can attend multiple courses)
