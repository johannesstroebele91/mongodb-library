# Basics

Is an more powerful alternative to the `find()` method

- builds a pipline of steps (e.g. `$match`, `$sort`, `$group`, ...)
- that runs on the data
- that is retrieved from the collection and
- provides the output in the specified form (a completely new document)

Important:

- steps can be combined
- steps can be reused
- steps can be repeated (multiple e.g. `$project` stage are possible)
  - so the data for the subsequent steps needs to be passed
  - from the previous step to the next one
  - by specifying them for each `$project` stage
- aggregation does not fetch alle the data from the database
  - so it is like the `find()` method+
  - which can be filtered
  - and only works with the filtered results in the next steps
- gives back an cursor

# Syntax

`db.someCollection.aggregate([{firstStep}, {secondStep}, ...])`

- `db` for referencing the current database
- `someCollection` creates a collection on the fly or uses an existing one
- `aggregate()` method which steps are passed as an array of object elements e.g. `$match`, `$sort`, `$group`, ...

All aggregation arguments like `$match` can be found here:
https://www.mongodb.com/docs/manual/core/aggregation-pipeline/

# $match

`$match` works similar to the `find()` method by

- filtering the documents to
- pass only the documents that
- match the specified condition(s) to the next pipeline stage.
- e.g. find only female concats and
  - returns all female contacts as a cursor
  - `db.persons.aggregate([{$match: {gender: "female"}}])`

# $group

`$group` groups the documents by item to retrieve distinct item values

- which is most often done by
- creating a new object variable to an `_id` key and
- processing it in some form
- e.g. the sum of people living in a certain state
- `db.persons.aggregate([{$match: {gender: "female"}}, {$group: {_id: {state: "$location.state"}, totalPersons: {$sum: 1}}}])`

# $sort

`$sort` does sort all input documents of the previous stage and

- returns them to the pipeline in sorted order and
- can use previously created variables like e.g. totalPersons and
- you can group by e.g. gender using `{$group: {_id: {gender: "$gender"}}`
- e.g. `db.persons.aggregate([{$match: {gender: "female"}}, {$group: {_id: {state: "$location.state"}, totalPersons: {$sum: 1}}}, {$sort: {totalPersons: -1}}])`
- e.g. get persons older than 50, grouped by gender,
  - amount of persons and average of age per,
  - sort by totalPersons
  - `db.persons.aggregate([{$match: {"dob.age": {$gt: 50}}}, {$group: {_id: {gender: "$gender"}, numPersons: {$sum: 1}, avgAge: {$avg: "$dob.age"}}}, {$sort: {numPersons: -1}}])`

# $project

`$project` is similar to projection of the `find()` method by

- taking a document that can specify
- inclusion of fields (e.g. `gender: 1`),
- suppression or exclusion of the fields (e.g. `_id: 0`),
- addition of new fields (e.g. `fullName: {$concat: [{$toUpper: "$name.first"}, " ", "$name.last", "hardCodedText"]}`), and
- resetting of the values of existing fields

Important: steps can be repeated (multiple e.g. `$project` stage are possible)

- so the data for the subsequent steps needs to be passed
- from the previous step to the next one
- by specifying them for each `$project` stage

## Example: `$concat`, `$toUpper`, `$substrCP`, `$subtract`, `$strLenCP`

```javascript
db.persons.aggregate([
  {
    $project: {
      _id: 0,
      gender: 1,
      fullName: {
        $concat: [
          { $toUpper: { $substrCP: ["$name.title", 0, 1] } },
          {
            $substrCP: [
              "$name.title",
              1,
              { $subtract: [{ $strLenCP: "$name.title" }, 1] },
            ],
          },
          " ",
          { $toUpper: "$name.first" },
          " ",
          "$name.last",
          "hardCodedText",
        ],
      },
    },
  },
]);
```

## Example `$convert` (e.g. into double or date)

PS ISODate() shown not up if used with mongosh shell

```javascript
db.persons.aggregate([
  {
    $project: {
      _id: 0,
      name: 1,
      gender: 1,
      email: 1,
      birthdate: { $convert: { input: "$dob.date", to: "date" } },
      age: "$dob.age",
      location: {
        type: "Point",
        coordinates: [
          {
            $convert: {
              input: "$location.coordinates.longitude",
              to: "double",
              onError: 0.0,
              onNull: 0.0,
            },
          },
          {
            $convert: {
              input: "$location.coordinates.latitude",
              to: "double",
              onError: 0.0,
              onNull: 0.0,
            },
          },
        ],
      },
    },
  },
  {
    $project: {
      _id: 0,
      gender: 1,
      email: 1,
      location: 1,
      birthdate: 1,
      age: 1,
      fullName: {
        $concat: [
          { $toUpper: { $substrCP: ["$name.title", 0, 1] } },
          {
            $substrCP: [
              "$name.title",
              1,
              { $subtract: [{ $strLenCP: "$name.title" }, 1] },
            ],
          },
          " ",
          { $toUpper: "$name.first" },
          " ",
          "$name.last",
          "hardCodedText",
        ],
      },
    },
  },
]);
```
