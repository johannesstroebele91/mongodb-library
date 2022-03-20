# Basics

1. Data is passed via e.g. insertOne(), updateOne() to change a collection
2. MongoDB will validate (accepts or rejects)
   - the passed data
   - based on the validation schema

MongoDB enables to define

- schema validation
- validation level
- validation action

# Validation level

Configuration possibilities:

- which kinds of operations get validated
- the documents which get validated
- the valuation level:
  - strict: all insert and updates are checked
  - moderate: all inserts are checked,
    - but updates only for documents
    - which were valid before

# Validation action

Configuration possibilities:

- what to do with validation errors
  - error: throw error and deny insert or update
  - warning: so proceed to change data, but log a warning

# HowToCreate collection

They can be created lazily (e.g. `db.posts.insertOne(...)`)

However, to set configuration options

- it needs to be explicitly created
- e.g. `db.createCollection()`
- which has the following arguments
  - 1. argument: name of the collection e.g. "posts"
  - 2. argument: configuration object
    - validator: define the schema

Example:

1. Create collection like in the example in the file `validation.js`
2. Add a document based on the schema `db.posts.insertOne({title: "Post", text: "desc", tags: ["new", "tech"], creator: ObjectId("6237395059b36fb055c61e13"), comments: [{text: "Post", author: ObjectId("6237395059b36fb055c61e13")}]})`
   - if a document is added that does not match the schema
   - an error with the errormsg: "document failed validation" appears
