# Basics

Reference: https://docs.mongodb.com/manual/reference/bson-types/

JSON (BSON) supports many data types such as:

- String (text): `{"someString": "file"}`
  - length has no maximum
  - maximum size is 16 mb
- Boolean: `{"someBoolean": true}`
  - true
  - false
- Number: `{"someNumber": 21312}`
  - IMPORTANT: mongosh and mongo shell and JS applications all are based on JS
    - therefore they can only store objects that JS can store
    - so numbers are only stored up to 17 digits
  - Integer (int32): 55 bits long (larger numbers will overflow)
  - NumberLong (int64): 10000000000
  - NumberDecimal: 12.99
  - Integer (int32): 55
- Array
  - Array with primitive data: `"someArray": [ "sports", "cooking" ]`
  - Array with objects: `"someArray": [ {"value": "New"}, {"value": "Open"}, ]`
- MongoDB ObjectId (BSON): `{"insertedIsd": ObjectId("1239sdfhjkaxca")}`
  - is created automatically when a new document is inserted
  - the type is specified via `ObjectId()`
  - it can be used for sorting
    - new objects will follow the order of creation time
    - so the automatically created timestamp explained below
- Time
  - ISODate: ISODate("2018-09-09"): is mostly used for time
    - Add the current date e.g. `{foundingDate: new Date()}`
    - Correct syntax is based on the driver
    - which is based coding language
    - and can be found here: https://docs.mongodb.com/drivers/node/current/
  - Timestamp: Timestamp(11421532): is created automatically internally
    - Add the current date e.g. `{insertedDate: new Timestamp()}`
    - Correct syntax is based on the driver
    - which is based coding language
    - and can be found here: https://docs.mongodb.com/drivers/node/current/
- Nested documents: `{"a": {...}}`

# Example

```JSON
{"someNestedObject": {
  "_id": ObjectId("62334d4722f77f96878bcb86"),
  "someString": "file",
  "someNumber": 21312,
  "someBoolean": true,
  "hello": {
    "someArray": [
      {"value": "New", "onclick": "CreateNewDoc()"},
      {"value": "Open", "onclick": "OpenDoc()"},
      {"value": "Close", "onclick": "CloseDoc()"}
    ]
  }
}}
```
