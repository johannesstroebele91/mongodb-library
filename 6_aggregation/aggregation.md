# Basics

Is an more powerful alternative to the `find()` method

- builds a pipline of steps (e.g. `$match`, `$sort`, `$group`, ...)
- that runs on the data
- that is retrieved from the collection and
- provides the output in the specified form (a completely new document)

Important:

- `$match` is the equivalent of the filter argument of the `find()` method
- steps can be combined
- steps can be reused
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

`$match` works similar to the `find()` method

- e.g. find only female concats `db.persons.aggregate([{$match: {gender: "female"}}])`
- returns all female contacts as a cursor

# $group

`$group` groups the documents by item to retrieve distinct item values

- which is most often done by
- creating a new object variable to an `_id` key and
- processing it in some form
- e.g. the sum of people living in a certain state
- `db.persons.aggregate([{$match: {gender: "female"}}, {$group: {_id: {state: "$location.state"}, totalPersons: {$sum: 1}}}])`
