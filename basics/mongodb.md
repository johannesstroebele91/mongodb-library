# Basics

MongoDB environment allows to

- create and start a MongoDB database server
- which allows to run
- multiple databases on it
- for creating, reding, updating, and deleting data
- using CRUD-operations

# Difference b/w MongoDB and other databases

NoSQL (Mongo DB) databases have

- collections and
- each of them has documents in JSON format, which look like JS objects
- which don't need to have a schema

SQL databases have

- tables and
- each of them has columns with rows
- based on a predefined schema (normalized)

# Relevance

It enables to work schemaless, so

- for apps that are still in development (without defined data requirements)
- you work with less relations (store data together),
  - so fetching data is faster
  - so no need to reach out to different collections and merge data

# Application areas

- Applications
  - e.g. user interacts with an database via an application
  - create, read, update, and delete of documents is often relevant
  - leverages MongoDB driver
- Analytics / BI tools
  - e.g. just get the data and want to analyse it
  - read is often relevant
  - leverages MongoDB BI connector / shell
- Admin
  - config a database
  - create, read, update, and delete of documents is often relevant
  - levergages MongoDB shell

All of these possibilities enable to

- interact with the MongoDB serer via a client
- which then handles the requests
- by adapting the collections and documents
- in the database
