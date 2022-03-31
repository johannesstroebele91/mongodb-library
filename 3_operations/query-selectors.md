**Basics**

Documents can be found using these query selectors:

- [comparison](#comparison)
- [logical](#logical)
- [element](#element)
- [evaluation](#evaluation)
- [array](#array)
- [comments](#comments)
- [geospatial](#geospatial)

## comparison

- `eq`: find documents which equal one value certain values, whereby equality differs for primitive data and arrays:
- primitive data e.g. `db.movies.findOne({runtime: {$eq: 60}})` is the same as `db.movies.findOne({runtime: 60})`
- arrays
  - contains the element e.g. `db.movies.findOne({genres: {$eq: "Drama"}})` is the same as `db.movies.findOne({genres: "Drama"})`
  - ONLY contains the element e.g. `db.movies.findOne({genres: {$eq: ["Drama"]}})` is the same as `db.movies.findOne({genres: ["Drama"]})`
- `ne`: find documents which equal NOT one value e.g. `db.movies.findOne({runtime: {$ne: 60}})`
- `in`: find documents which equal MULTIPLE values e.g. `db.movies.findOne({runtime: {$in: [30, 42]}})`
- `nin`: find documents which NOT equal MULTIPLE values e.g. `db.movies.findOne({runtime: {$nin: [30, 42]}})`
- `gt`: greater then e.g. `db.movies.findOne({runtime: {$gt: 60}})`
- `gte`: greater then OR equals e.g.`db.movies.findOne({runtime: {$gte: 60}})`
- `lt`: lower then e.g. `db.movies.findOne({runtime: {$lt: 60}})`
  - `lte`: lower then OR equals e.g. `db.movies.findOne({runtime: {$lt: 60}})`

## logical

- `or`: documents that match any of the conditions
  - primitive data e.g. `db.movies.find({$or: [{"rating.average": {$lt: 5}}, {"rating.average": {$gt: 9.3}}]})`
  - arrays e.g. `db.users.find({$or: [{"hobbies.title": "Yoga"}, {"hobbies.title": "Sports"}]})`
- `nor`: documents that DON'T match any of the conditions e.g. `db.movies.find({$nor: [{"rating.average": {$lt: 5}}, {"rating.average": {$gt: 9.3}}]})`
- `and`: all found documents must match elements of the array
  - IMPORTANT:
    - `db.movies.find({"rating.average": {$gt: 9}, genres: "Drama"})` is in most cases as
    - the same as `db.movies.find({$and: [{"rating.average": {$gt: 9}}, {genres: "Drama"}]})`
  - BUT `$and` solution is more secure
    - because if properties are used multiple times in a filter
    - e.g. `db.movies.find({"rating.average": {$gt: 9}, genres: "Drama"})`
    - ONLY the last mention of the property is used!!!
    - SO in that case `$and` NEEDS to be used
- `not`: find documents that include not a certain value e.g. `db.movies.find({runtime: {$not: {$eq: 60}}})`
  - is more complex, and can mostly be substituted with easier statements using `$ne`
  - e.g. `db.movies.find({runtime: {$ne: 60}})`

## element

- `exists`: finds all documents that have an element that exists e.g. `db.users.find({age: {$exists: true}})`
  - IMPORTANT
    - an element is included although it might have the value `null`
    - e.g.`db.users.find({age: {$exists: false, $ne: null}})`
- `type`: e.g. "string", "double" (e.g. 2.42, 0213), "int" (e.g. 7, 32)
  - finds all documents which element have a certain **type** e.g. `db.users.find({phone: {$type: "string"}})`
  - finds all documents which element have certain **types** e.g. `db.users.find({phone: {$type: ["int", "string"]}})`

## evaluation

- `regex`: allows to search for text in a key-value pair of a document
  - e.g. normal search like `db.movies.find({summary: "musical"})` only looks for full equality!!
  - regex enables to search for parts of text in a key-value pair e.g. `db.movies.find({summary: {$regex: /musical/}})`
    - a regex expression is always surrounded by `//`
    - and have in between e.g. `/musical/`
- `expr`: expression find documents where the comparison of two fields inside of the respective document equals true
  - e.g. where the volume is greater than the target for financial data `db.sales.find({$expr: {$gt: ["$volume", "$target"]}})`
  - e.g. where the number of visitors exceed the expected visitors e.g. `db.movieStars.find({$expr: {$gt: ["$visitors", "$expectedVisitors"]}})`
  - to access the value of the key-value pairs `"$someKey"` is needed
  - e.g. `db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: 190}}]}})`
  - e.g. `db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: {$subtract: ["$volume", 10]}, else: "$volume"}}]}})`
  - SHORT FORM: e.g. `db.sales.find({name: /60795a5eb9d2aa55101a4006/})`

## array

- `size`: find docus with exact amount of elements in an nested array
  - until now, MongoDB only supports exact sizes, no $gt or $lt
  - e.g. `db.users.find({hobbies: {$size: 3}})`
- `all`: find docus that have arrays that
  - ONLY include the following elements in a unsorted order
  - e.g. `db.movieStars.find({genre: {$all: ["action", "thriller"]}})`
  - which is in contrast to
    - .g. `db.movieStars.find({genre: ["action", "thriller"]})`
    - where the order of elements in an array is important
- `elemMatch`: find documents which elements look a certain way
  - for arrays with primitive data as elements e.g. `db.exmoviestars.find({ratings: {$elemMatch: {$gt: 8, $lt: 10}}})`
  - for arrays with objects as elements e.g. `db.users.find({hobbies: {$elemMatch: {title: "Sports", frequency: {$gte: 3}}}})`
    - why not use `ànd`?
      - because it cannot control that
      - both conditions need to be met by
      - ONLY one element of an array
      - e.g. `db.users.find({$and: [{"hobbies.title": "Sports"}, {"hobbies.frequency": {$gte: 3}}]})`

## comments

- ``:

## geospatial

- e.g. near some location
- ``:
