**Table of Contents**

- [1. Basics](#1-basics)
- [2. Syntax](#2-syntax)
- [3. Stages](#3-stages)
- [4. Apply multiple operations to an array](#4-apply-multiple-operations-to-an-array)
- [5. Using additional stages](#5-using-additional-stages)

# 1. Basics

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

# 2. Syntax

`db.someCollection.aggregate([{firstStep}, {secondStep}, ...])`

- `db` for referencing the current database
- `someCollection` creates a collection on the fly or uses an existing one
- `aggregate()` method which steps are passed as an array of object elements e.g. `$match`, `$sort`, `$group`, ...

All aggregation arguments like `$match` can be found here:
https://www.mongodb.com/docs/manual/core/aggregation-pipeline/

# 3. Stages

Stages, such as `$match`, `$sort`, `$group`, `$project`

- enable to work with complex data
- by passing it through a pipeline
- and are explained in the file `stages.md`

# 4. Apply multiple operations to an array

```javascript
db.friends.aggregate([
  { $unwind: "$examScores" },
  { $project: { _id: 1, name: 1, age: 1, score: "$examScores.score" } },
  { $sort: { score: -1 } },
  {
    $group: {
      _id: "$_id",
      name: { $first: "$name" },
      maxScore: { $max: "$score" },
    },
  },
  { $sort: { maxScore: -1 } },
]);
```

# 5. Using additional stages

IMPORTANT: ORDER is important!!!

```javascript
db.persons.aggregate([
  { $match: { gender: "male" } },
  {
    $project: {
      _id: 0,
      name: { $concat: ["$name.first", " ", "$name.last"] },
      birthdate: { $toDate: "$dob.date" },
    },
  },
  { $sort: { birthdate: 1 } },
  { $skip: 10 },
  { $limit: 10 },
]);
```
