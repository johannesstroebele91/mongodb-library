# Basics

Reference: https://www.mongodb.com/docs/manual/core/aggregation-pipeline/

Is an more powerful alternative to the `find()` method

- builds a pipline of steps (e.g. `$match`, `$sort`, `$group`, )
- that runs on the data
- that is retrieved from the collection and
- provides the output in the specified form

Important:

- `$match` is the equivalent of the filter argument of the `find()` method
- steps can be combined
- steps can be reused
