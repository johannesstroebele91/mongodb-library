# 1. `$concat`, `$toUpper`, `$substrCP`, `$subtract`, `$strLenCP`, `$convert: { input: "$dob.date", to: "someDataTye" }`

PS ISODate() shown not up if used with mongosh shell

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

# 2. `$toDate`, `$toInt`, `$isoWeekYear`

```javascript
db.persons.aggregate([
  {
    $project: {
      _id: 0,
      name: 1,
      gender: 1,
      email: 1,
      birthdate: { $convert: { input: "$dob.date", to: "date" } },
      birthYear: { $isoWeekYear: "$birthdate" },
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

# 3. `$push` & `$addToSet` pushes elements into a newly grouped array

1. `$push` push all elements into a newly groupped array **including duplicates**

```javascript
db.friends.aggregate([
  { $group: { _id: { age: "$age" }, allHobbies: { $push: "$hobbies" } } },
]);
```

2. `$addToSet` only add **unique elements** into the new array

```javascript
db.friends.aggregate([
  { $unwind: "$hobbies" },
  { $group: { _id: { age: "$age" }, allHobbies: { $addToSet: "$hobbies" } } },
]);
```

# 4. Using projection with arrays with `$slice`, `$size`

`$slice` returns a subset of an array by passing arguments

1. array
2. amount of elements that should be returned
   - return a certain amount
     - e.g. 1 element starting from the first element in the array
     - `db.friends.aggregate([{$project: { _id: 0, examScore: {$slice: ["$examScores", 1]}}}])`
   - return the last
     - e.g. 2 elements
     - `db.friends.aggregate([{$project: { _id: 0, examScore: {$slice: ["$examScores", -2,2]}}}])`
   - return a range of elements
     - e.g. `db.friends.aggregate([{$project: { _id: 0, examScore: {$slice: ["$examScores", 2,2]}}}])`
       - returns starting from the 3rd element (due to starting at index 0)
       - 2 elements

`$size` the length of an array (starting at 1)

```javascript
db.friends.aggregate([
  { $project: { _id: 0, examScore: { $size: "$examScores" } } },
]);
```

# 5. Filter out certain array element `$filter`

- input: array
- as: variable
- cond: condition

```javascript
db.friends.aggregate([
  {
    $project: {
      _id: 0,
      scores: {
        $filter: {
          input: "$examScores",
          as: "sc",
          cond: { $gt: ["$$sc", 60] },
        },
      },
    },
  },
]);
```

# 6. Apply multiple operations to an array

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
