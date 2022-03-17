# Basics

JSON is a lightweight data-interchange format

- used by MongoDB and
- easily readable by humans
- to store and transmit data objects
- consisting of attribute–value pairs and arrays

# Types

JSOn supports many data types such as:

- String: `{"someString": "file"}`
- Number: `{"someNumber": 21312}`
- Boolean: `{"someBoolean": true}`
- Array: `"someArray": [ {"value": "New"}, {"value": "Open"}, ]`
- Exception MongoDB ObjectId: `{"insertedId": ObjectId("1239sdfhjkaxca")}`
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
