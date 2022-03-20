# Basics

JSON (BSON) is a lightweight data-interchange format

- used by MongoDB and
- easily readable by humans
- to store and transmit data objects
- consisting of attribute–value pairs and arrays

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

# JSON vs BSON

MongoDB uses BSON behind the scenes

- which strands for binary JSON
- to store data in a database

The conversion behind the scenes into JSON

- is done by MongoDB drivers
- which takes the JSON code and
- stores it in binary format

The reason for this is that it is

- more efficient to store than JSON (faster, less size) and
- supports additional types (e.g. OBjectId)
