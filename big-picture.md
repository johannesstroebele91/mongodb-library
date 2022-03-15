# Basics

## 1. Trigger database requests

Database requests can be triggered via two general methods:

### 1.1. Application

- Frontend: views or UI where user can trigger request
- Backend: server-side logic which handels request
  - MongoDB Drivers
    - e.g. for Node.js
    - interacts with the MongoDB server via query

### 1.2. Application

MongoDB Shell enables to

- write query
- playground to experiment with the database
- administer the database (configuration)

## 2. Data

The requests are handled as follows:

- MongoDB server: passes the request form the server to the storage engine
- Storage engine (WiredTiger for file / data access):
  - makes changes to the database files OR
  - gets data from the database
  - based on the requests from the server
- Database: data is stored in files via collections and documents
