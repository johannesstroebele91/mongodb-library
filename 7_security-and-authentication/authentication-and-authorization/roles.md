# Basics

Users by default have no privileges AND

- need to be assigned roles
- to get them

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

# Built in roles

Ref (shows what users of built-in roles can do with them): https://www.mongodb.com/docs/manual/reference/built-in-roles/

_Sorted from less to most power for each group_

- Database users
  - user that need to work with only one database in one MongoDB
  - `read`: e.g. not only `find()` cmd but `listCollection()`, ...
  - `readWrite`
- Database admin
  - user that need to work with only one database in one MongoDB
  - `dbAdmin`: manage database, create collections
  - `userAdmin`: creating and managing users
  - `dbOwner`: handels database
- All database roles
  - user that need to work with all databases in one MongoDB
  - `readAnyDatabase`
  - `readWriteAnyDatabase`
  - `userAdminAnyDatabase`
  - `dbAdminAnyDatabase`
- Cluster admin
  - users that need to work with a cluster
  - a cluster consists of multiple MongoDB servers working together
  - `clusterManager`
  - `clusterMonitor`
  - `hostManager`
  - `clusterAdmin`
- backup/ restore
  - `backup`
  - `restore`
- Superuser (have all privileges)
  - `dbOwner` (admin)
  - `userAdmin` (admin)
  - `userAdminAnyDatabase`
  - `root` (most powerful, so the user as privileges like without authentication)
