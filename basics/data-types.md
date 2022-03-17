# Basics

JSON (BSON) supports many data types such as:

- String: `{"someString": "file"}`
- Number: `{"someNumber": 21312}`
- Boolean: `{"someBoolean": true}`
- Array: `"someArray": [ {"value": "New"}, {"value": "Open"}, ]`
- MongoDB ObjectId (BSON): `{"insertedIsd": ObjectId("1239sdfhjkaxca")}`
  - is created automatically when a new document is inserted
  - the type is specified via `ObjectId()`
  - it can be used for sorting (includes a timestamp)

# Example

```JSON
{"someNestedObject": {
  "id": "1239sdfhjkaxca",
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
