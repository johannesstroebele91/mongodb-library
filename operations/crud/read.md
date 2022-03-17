# Basic

Get one or multiple documents

- in a collection
- e.g. `db.products.find(filter, options)`
- by passing the parameters:
  - Filter: narrow down which documents to change
  - Options: configuration

# find()

Get one or multiple documents

- syntax `db.products.find(filter, options)`
- find all documents in a collection e.g. `db.products.find()`
- find one or multiple documents in a collection
  - that match the filter criteria
  - e.g. `flightData.find({intercontinental: true})`
- find alle documents where the filter criteria
  - is e.g. greater than something `distance: {$gt: 10000}`
  - e.g. `db.flightData.find({distance: {$gt: 10000}})`

Important:

- all documents are returned by passing no parameter
- a nicer format for an output can be archived via `db.products.find().pretty()`

# findOne()

Get the first matching document it finds

- syntax `db.products.findOne(filter, options)`
- e.g. `db.bios.findOne({ name: 1, contribs: 1 } )`
