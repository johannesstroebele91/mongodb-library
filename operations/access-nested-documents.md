# Find

Nested documents can be accessed by

- find one document and returning a certain field `db.passengers.findOne({name: "Albert Twostone"}).hobbies`
- find all documents that have a certain field `db.passengers.find({hobbies: "sports"})`
  - MongoDB is smart enough to know
  - which data type is specified
  - e.g. `hobbies` is an array that can have `sports`
- find all documents hat have a nested field `db.flightData.find({"status.description": "on-time"})`
  - the nested field needs to be
  - wrapped in "" like `"status.description"`

# Update

Nested documents can be updated using

`db.patients.updateOne({firstName: "Max"}, {$set: {lastName: "Peterson", age: 52, history: [{disease: "very sad", treatment: "4rwefds"}]}})`

# Delete

Nested documents can be deleted using

`db.patients.deleteMany({"history.disease": "cold"})`
