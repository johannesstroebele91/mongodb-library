_The following operators enable to update matched elements in an array_

# Adding an element or elements to an array

- `push` enables to push a new element onto an array
  - no matter if the element already exists
  - e.g. `db.users.updateOne({name: "Maria"}, {$push: {hobbies: {title: "Sports", frequency: 2}}})`
- `addToSet` enables to push a new element onto an array,
  - if the element is unique (does not already exist in the array)
  - e.g. `db.users.updateOne({name: "Maria"}, {$addToSet: {hobbies: {title: "Hiking", frequency: 2}}})`
- `each` enables to push multiple elements onto an array
  - which can be also e.g. sorted afterwards
  - e.g. `db.users.updateOne({name: "Maria"}, {$push: {hobbies: {$each: [{title: "Good wine", frequency: 1}, {title: "Hiking", frequency: 2}], $sort: {frequency: -1}, $slice: 1}}})`

# Removing an element\*\* elements from an array

- `pull` enables to remove an specific element from an array
  - e.g. `db.users.updateOne({name: "Maria"}, {$pull: {hobbies: {title: "Hiking"}}}`
- `pop` enables two things
  - value `1` removes the last element e.g. `db.users.updateOne({name: "Chris"}, {$pop: {hobbies: 1}})`
  - value `-1` removes the first element e.g. `db.users.updateOne({name: "Chris"}, {$pop: {hobbies: 1}})`

# Work more easily with arrays

**Use $, $[], $[<identifier>] + arrayfilter** to work more easily with arrays

- `.$` enables to overwrite the first element in an array
  - that matches the filter criteria
  - e.g. `db.users.updateMany({hobbies: {$elemMatch: {title: "Sports", frequency: {$gte: 4}}}}, {$set: {"hobbies.$": {title: "Sports", frequency: 2}}})`
- `array.$.newFieldName` enables to adding a new field
  - to the first element in an array
  - that matches the filter condition
  - e.g. `db.users.updateMany({hobbies: {$elemMatch: {title: "Sports", frequency: {$gte: 4}}}}, {$set: {"hobbies.$.highFrequency": true}})`
- `array.$[].newFieldName` enables to adding a new field
  - to all elements in an array
  - that matches the filter condition
  - e.g. `db.users.updateMany({age: {$gt: 30}}, {$inc: {"hobbies.$[].frequency": -1}})`
- `array.$[el].newFieldName /// arrayFilters: [{el: "someValue"}]`
  - enables to adding a new field
  - to specific elements in an array
  - that match the filter condition
  - by creating a variable e.g. `el` and
  - using int in the `arrayFilter` option
  - e.g. `db.users.updateMany({"hobbies.frequency": {$gt: 2}}, {$set: {"hobbies.$[el].goodFrequency": true}}, {arrayFilters: [{"el.frequency": {$gt: 2}}]})`
