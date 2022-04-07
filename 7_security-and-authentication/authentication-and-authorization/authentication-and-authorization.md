# Basics

MongoDB uses

- authentication and
- authorization
- as explained below

The users can authenticated themselves via:

- shell (e.g. mongo, mongosh)
- application which uses the MongoDB driver (e.g. React)

# Authentication

Identifies valied users of the database

Analogy:

- you are employed
- so you get a keycard
- to can access the office

# Authorization

Identifies what these users can do in the database

Analogy:

- you role in the company is an accountant
- so you get access to the financial data
- in the office

# Example MongoDB server

- shop database
  - products collection
  - customers collection
- blog database
  - posts collection
  - authors collection
- admin database
  - special database
  - which exists out of the box

# Example authentication and authorization in practice

To login to the the MongoDB server

- a users needs to authenticate themselves
- by passing
  1. username
  2. password

However, authentication by itself does not

- allow users to do anything
- on the MongoDB server

Users have

- roles that enable them to do stuff
- which are essentially groups of privileges

Privilege is a combination of

1. resource: e.g. products collection in the shop database
2. action: e.g. insert()

Database admins have the task to

- assign users roles
- in order to enable them to
- work with the MongoDB server
- e.g. developer, viewer
