**Table of Contents**

- [Basics](#basics)
- [Nested / embedded documents](#nested--embedded-documents)
  - [1. Add document to the patients collection with the diseaseSummary data](#1-add-document-to-the-patients-collection-with-the-diseasesummary-data)
- [References (like SQL)](#references-like-sql)
  - [1. Add document to patients collection](#1-add-document-to-patients-collection)
  - [2. Add document to diseaseSummaries collection](#2-add-document-to-diseasesummaries-collection)
  - [3. Return the id of the diseaseSummary of the patient](#3-return-the-id-of-the-diseasesummary-of-the-patient)
- [Relation types](#relation-types)

# Basics

Multiple collections

- can be related
- which can be done using:

# Nested / embedded documents

- Pro: only one query needed to fetch all the user data
- Contra: might lead to lots of duplicated code, which is harder to handle if it changes

## 1. Add document to the patients collection with the diseaseSummary data

`db.patients.insertOne({name: "Max", age: 29, diseaseSummary: {diseases: ["cold", "broken leg"]}})`

Output:

``

# References (like SQL)

Links like the ObjectId

- can be used in collections
- to reference the document in one collection
- to a document in another one

- Pro: easier to handle if the data in one of the collection changes
- Contra: multiple queries needed to fetch all user data

## 1. Add document to patients collection

`db.patients.insertOne({name: "Max", age: 29, diseaseSummary: "summary-max-1"})`

Output:

```BSON
{ _id: ObjectId("62348a4a679fb2f726d1ccad"),
  name: 'Max',
  age: 29,
  diseaseSummary: { diseases: [ 'cold', 'broken leg' ] } }
```

## 2. Add document to diseaseSummaries collection

`db.diseaseSummaries.insertOne({_id: "summary-max-1", diseases: ["cold", "broken leg"]})`

Output:

```json
{ "_id": "summary-max-1", "diseases": ["cold", "broken leg"] }
```

## 3. Return the id of the diseaseSummary of the patient

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
