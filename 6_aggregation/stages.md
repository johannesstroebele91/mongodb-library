**Table of Contents**

Stages, such as `$match`, `$sort`, `$group`, `$project`

- enable to work with complex data
- by passing it through a pipeline

- [1. $match](#1-match)
- [2. $sort](#2-sort)
- [3. $group](#3-group)
- [4. $unwind](#4-unwind)
- [5. $project](#5-project)
- [6. `$bucket` for analyzing data](#6-bucket-for-analyzing-data)
- [7 `$skip` and `$limit` to control amount of results](#7-skip-and-limit-to-control-amount-of-results)
- [8 `$out` to write results of a pipeline to new collection](#8-out-to-write-results-of-a-pipeline-to-new-collection)

# 1. $match

`$match` works similar to the `find()` method by

- filtering the documents to
- pass only the documents that
- match the specified condition(s) to the next pipeline stage.
- e.g. find only female concats and
  - returns all female contacts as a cursor
  - `db.persons.aggregate([{$match: {gender: "female"}}])`

# 2. $sort

`$sort` does sort all input documents of the previous stage and

- returns them to the pipeline in sorted order and
- can use previously created variables like e.g. totalPersons and
- you can group by e.g. gender using `{$group: {_id: {gender: "$gender"}}`
- e.g. `db.persons.aggregate([{$match: {gender: "female"}}, {$group: {_id: {state: "$location.state"}, totalPersons: {$sum: 1}}}, {$sort: {totalPersons: -1}}])`
- e.g. get persons older than 50, grouped by gender,
  - amount of persons and average of age per,
  - sort by totalPersons
  - `db.persons.aggregate([{$match: {"dob.age": {$gt: 50}}}, {$group: {_id: {gender: "$gender"}, numPersons: {$sum: 1}, avgAge: {$avg: "$dob.age"}}}, {$sort: {numPersons: -1}}])`

# 3. $group

`$group` groups multiple documents into one document (n:1 relation) by

- creating a new object variable to an `_id` key and
- processing it in some form
- without an alias e.g. `db.friends.aggregate([{$group: {_id: "$age"}])`
- with an alias e.g. `db.friends.aggregate([{$group: {_id: {age: "$age"}}}])`
- e.g. the sum of people living in a certain state
  - `db.persons.aggregate([{$match: {gender: "female"}}, {$group: {_id: {state: "$location.state"}, totalPersons: {$sum: 1}}}])`

# 4. $unwind

`$unwind` takes one document with an array and

- creates multiple documents
- for each element in the array

Example before unwind

```javascript
{ _id: ObjectId("624c413f83353779d703fc6c"),
  name: 'Maria',
  hobbies: [ 'Cooking', 'Skiing' ],
  age: 29,
  examScores:
   [ { difficulty: 3, score: 75.1 },
     { difficulty: 8, score: 44.2 },
     { difficulty: 6, score: 61.5 } ] }
```

Example after unwind: `db.friends.aggregate([{$unwind: "$hobbies"}])"`

```javascript
{ _id: ObjectId("624c413f83353779d703fc6c"),
  name: 'Maria',
  hobbies: 'Cooking',
  age: 29,
  examScores:
   [ { difficulty: 3, score: 75.1 },
     { difficulty: 8, score: 44.2 },
     { difficulty: 6, score: 61.5 } ] }
{ _id: ObjectId("624c413f83353779d703fc6c"),
  name: 'Maria',
  hobbies: 'Skiing',
  age: 29,
  examScores:
   [ { difficulty: 3, score: 75.1 },
     { difficulty: 8, score: 44.2 },
     { difficulty: 6, score: 61.5 } ] }
```

# 5. $project

`$project` is similar to projection of the `find()` method by

- getting one document and returning it (1:1 relation)
- taking a document that can specify
- inclusion of fields (e.g. `gender: 1`),
- suppression or exclusion of the fields (e.g. `_id: 0`),
- addition of new fields (e.g. `fullName: {$concat: [{$toUpper: "$name.first"}, " ", "$name.last", "hardCodedText"]}`), and
- resetting of the values of existing fields

Important: steps can be repeated (multiple e.g. `$project` stage are possible)

- so the data for the subsequent steps needs to be passed
- from the previous step to the next one
- by specifying them for each `$project` stage

**Results of projections can be grouped**

```javascript
db.persons
  .aggregate([
    {
      $project: {
        _id: 0,
        name: 1,
        email: 1,
        birthdate: { $toDate: "$dob.date" },
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
        gender: 1,
        email: 1,
        location: 1,
        birthdate: 1,
        age: 1,
        fullName: {
          $concat: [
            { $toUpper: { $substrCP: ["$name.first", 0, 1] } },
            {
              $substrCP: [
                "$name.first",
                1,
                { $subtract: [{ $strLenCP: "$name.first" }, 1] },
              ],
            },
            " ",
            { $toUpper: { $substrCP: ["$name.last", 0, 1] } },
            {
              $substrCP: [
                "$name.last",
                1,
                { $subtract: [{ $strLenCP: "$name.last" }, 1] },
              ],
            },
          ],
        },
      },
    },
    {
      $group: {
        _id: { birthYear: { $isoWeekYear: "$birthdate" } },
        numPersons: { $sum: 1 },
      },
    },
    { $sort: { numPersons: -1 } },
  ])
  .pretty();
```

# 6. `$bucket` for analyzing data

```javascript
db.friends.aggregate([
  {
    $bucketAuto: {
      groupBy: "$dob.age",
      buckets: 5,
      output: { numPersons: { $sum: 1 }, averageAge: { $avg: "$dob.age" } },
    },
  },
]);
```

```javascript
db.persons.aggregate([
  {
    $bucket: {
      groupBy: "$dob.age",
      boundaries: [18, 30, 40, 50, 60, 120],
      output: {
        numPersons: { $sum: 1 },
        averageAge: { $avg: "$dob.age" },
      },
    },
  },
]);
```

# 7 `$skip` and `$limit` to control amount of results

IMPORTANT:mostly skip should be used before limit

```javascript
db.persons.aggregate([
  {
    $project: {
      _id: 0,
      name: { $concat: ["$name.first", " ", "$name.last"] },
    },
  },
  { $skip: 10 },
  { $limit: 10 },
]);
```

# 8 `$out` to write results of a pipeline to new collection

```javascript
db.persons.aggregate([
  {
    $project: {
      _id: 0,
      name: 1,
      email: 1,
      birthdate: { $toDate: "$dob.date" },
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
      gender: 1,
      email: 1,
      location: 1,
      birthdate: 1,
      age: 1,
      fullName: {
        $concat: [
          { $toUpper: { $substrCP: ["$name.first", 0, 1] } },
          {
            $substrCP: [
              "$name.first",
              1,
              { $subtract: [{ $strLenCP: "$name.first" }, 1] },
            ],
          },
          " ",
          { $toUpper: { $substrCP: ["$name.last", 0, 1] } },
          {
            $substrCP: [
              "$name.last",
              1,
              { $subtract: [{ $strLenCP: "$name.last" }, 1] },
            ],
          },
        ],
      },
    },
  },
  { $out: "transformedPersons" },
]);
```
