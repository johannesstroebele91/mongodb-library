# Relevance

Often, applications only need

- a fraction of the data
- from a database

Projection enables to

- return only a certain field or fields
- so not filtering but specifying data

Advantages of projection are

- the improved performance
- because the data transformation
- happens in the server
- and not in the client

# Example

Specify which key-value pairs are included

- the id is by default included (can be omitted using `_id: 0`)
- and all other key-value pairs are omitted by default
- `db.passengers.find({}, {name: 1, _id: 0})`

Returned data:

```BSON
{ _id: ObjectId("623356a022f77f96878bcb99"),
  name: 'Klaus Arber' },
{ _id: ObjectId("623356a022f77f96878bcb9a"),
  name: 'Albert Twostone' `}
```

Data in a database

```JSON
{
    "_id": "7148wxhcj",
    "name": "Max",
    "age": 29,
    "job": "instructor",
}
```

Needed data in application

```JSON
{
    "name": "Max",
    "age": 29,
}
```
