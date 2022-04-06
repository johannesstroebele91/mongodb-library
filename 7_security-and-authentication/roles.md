# Example of typical DB users

Different types of database users are:

- administrator:
  - SHOULD BE ABLE: manage the database config, create users, ...
  - SHOULD'NT: insert or fetch data from the MongoDB
- developer / your app:
  - SHOULD BE ABLE: insert, update, delete, and fetch data (CRUD)
  - SHOULD'NT: manage the database config, create users, ...
- data scientist
  - SHOULD BE ABLE: fetch data
  - SHOULD'NT: edit or delete data, manage the database config, ...
