**Table of Contents**

- [Basics](#basics)
- [Validation level](#validation-level)
- [Validation action](#validation-action)
- [HowToCreate collection with validation](#howtocreate-collection-with-validation)
- [HowToChange validation of a collection](#howtochange-validation-of-a-collection)

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

# HowToCreate collection with validation

They can be created lazily (e.g. `db.posts.insertOne(...)`)

However, to set configuration options

- it needs to be explicitly created
- e.g. `db.createCollection()`
- which has the following arguments
  - 1. argument: name of the collection e.g. "posts"
  - 2. argument: configuration object
    - validator: define the schema

Example:

1. Create collection like:
   ```bson
   db.createCollection("posts", {
    validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["title", "text", "creator", "comments"],
      properties: {
        title: {
          bsonType: "string",
          description: "must be a string and is required",
        },
        text: {
          bsonType: "string",
          description: "must be a string and is required",
        },
        creator: {
          bsonType: "objectId",
          description: "must be an objectid and is required",
        },
        comments: {
          bsonType: "array",
          description: "must be an array and is required",
          items: {
            bsonType: "object",
            required: ["text", "author"],
            properties: {
              text: {
                bsonType: "string",
                description: "must be a string and is required",
              },
              author: {
                bsonType: "objectId",
                description: "must be an objectid and is required",
                  },
                },
              },
            },
          },
        },
      },
    });
   ```
2. Add a document based on the schema `db.posts.insertOne({title: "Post", text: "desc", tags: ["new", "tech"], creator: ObjectId("6237395059b36fb055c61e13"), comments: [{text: "Post", author: ObjectId("6237395059b36fb055c61e13")}]})`
   - if a document is added that does not match the schema
   - an error with the errormsg: "document failed validation" appears

# HowToChange validation of a collection

The validation can be changed

- using `db.runCommand()` like:
  ```bson
  db.runCommand({
  collMod: "posts",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["title", "text", "creator", "comments"],
      properties: {
        title: {
          bsonType: "string",
          description: "must be a string and is required",
        },
        text: {
          bsonType: "string",
          description: "must be a string and is required",
        },
        creator: {
          bsonType: "objectId",
          description: "must be an objectid and is required",
        },
        comments: {
          bsonType: "array",
          description: "must be an array and is required",
          items: {
            bsonType: "object",
            required: ["text", "author"],
            properties: {
              text: {
                bsonType: "string",
                description: "must be a string and is required",
              },
              author: {
                bsonType: "objectId",
                description: "must be an objectid and is required",
              },
            },
          },
        },
      },
    },
  },
  validationAction: "warn",
  });
  ```
- which has the arguments:
  - 1. argument: `collMod` which is the collection that should be modified
  - 2. argument: validator can edited here
  - 3. argument: validationLevel can be either "error" or "warn"
    - the log for the warn option is stored
    - in the log files
