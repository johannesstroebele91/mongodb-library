**Table of Contens**

- [Relevance](#relevance)
- [New example](#new-example)
- [Examples for primitive data](#examples-for-primitive-data)
  - [One field](#one-field)
  - [One field and omitting the id](#one-field-and-omitting-the-id)
  - [Multiple fields](#multiple-fields)
  - [Nested field](#nested-field)

# Relevance

Projection enables to shape data, so to

- return only a certain field or fields
- so not filtering but specifying data

Often, applications only need

- a fraction of the data
- from a database

Advantages of projection are

- the improved performance
- because the data transformation
- happens in the server
- and not in the client

# New example

Projection helps to only return certain fields

- of a complicated object by
- if one or more fields are specified
  - for the second argument of `find({}, {name: 1})`,
  - all other fields are automatically omitted
  - expect the `_id` field
    - which is included by default
    - BUT it can be also omitted using `_id: 0`
    - `db.passengers.find({}, {name: 1, _id: 0})`

If the field is included or not is handled using the value

- include field: value `1`
- omit field: value `0`

IMPORTANT: **unnecessary to skip other fields** (are automatically omitted)

# Examples for primitive data

## One field

`db.movies.findOne({}, {name: 1})`

Output:

```BSON
{ _id: ObjectId("6238d40ca85864906e63df87"), name: 'Arrow' }
```

## One field and omitting the id

`db.movies.findOne({}, {name: 1, _id: 0})`

Output:

```BSON
{ name: 'Arrow' }
```

## Multiple fields

`db.movies.findOne({}, {name: 1, runtime: 1, genres: 1})`

```BSON
{ _id: ObjectId("6238d40ca85864906e63df87"),
  name: 'Arrow',
  genres: [ 'Drama', 'Action', 'Science-Fiction' ] }
```

## Nested field

`db.movies.findOne({}, {"rating.average": 1, _id: 0})`

```BSON
{ rating: { average: 8 } }
```
