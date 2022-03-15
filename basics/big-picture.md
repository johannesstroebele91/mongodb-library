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

## 2. Handle requests

The requests are handled as follows:

- MongoDB server:
  - passes the request form the server to the storage engine and
  - serves the databas
  - is hosted via
    - community server (free), enterprise server (for security & config) OR
    - atlas (cloud): the cloud manager enables to do ops stuff
- Storage engine (WiredTiger for file / data access):
  - makes changes to the database files OR
  - gets data from the database
  - based on the requests from the server
- Database: files where the data is stored in via collections and documents

## 3. Visualize data

- Compass: graphical UI which allow to connect to database an look into data more conveniently
- BI connectors: allow to connect to different BI tools
- MongoDB charts: visualize data
